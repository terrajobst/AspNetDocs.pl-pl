---
uid: web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
title: Korzystanie z narzędzia Page Inspector w programie Visual Studio 2012 | Microsoft Docs
author: rick-anderson
description: W ramach tego praktycznego laboratorium zostanie wykryte nowe narzędzie do znajdowania i naprawiania problemów ze stronami sieci Web w programie Visual Studio — Inspektor strony. Page Inspector to nowe narzędzie, które b...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 73232292-a5fe-4720-82a1-8f6553effd1f
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: f42b1be2697ba7d1145b3e334fe8f4ebf019cd12
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78586255"
---
# <a name="using-page-inspector-in-visual-studio-2012"></a>Korzystanie z narzędzia Page Inspector w programie Visual Studio 2012

przez [zespół Camp sieci Web](https://twitter.com/webcamps)

> W ramach tego praktycznego laboratorium zostanie wykryte nowe narzędzie do znajdowania i naprawiania problemów ze stronami sieci Web w programie Visual Studio — Inspektor strony.
> 
> Page Inspector to nowe narzędzie, które oferuje narzędzia do diagnostyki przeglądarki w programie Visual Studio i zapewnia zintegrowane środowisko w przeglądarce, ASP.NET i kodzie źródłowym. Renderuje stronę sieci Web (HTML, Web Forms, ASP.NET MVC lub Web Pages) bezpośrednio w środowisku IDE programu Visual Studio i umożliwia badanie kodu źródłowego oraz wynikowych danych wyjściowych. Inspektor strony pozwala łatwo rozłożyć witrynę internetową, szybko kompilować strony od podstaw oraz szybko diagnozować problemy.
> 
> Obecnie mamy wiele platform sieci Web, które tworzą elastyczne i skalowalne witryny internetowe w odpowiednim czasie, takie jak ASP.NET MVC i WebForms. Z drugiej strony trudniejsze jest znalezienie problemów na stronach, ponieważ IDE nie obsługuje widoku projektanta na stronach opartych na szablonach i zawartości dynamicznej. W związku z tym te witryny sieci Web muszą być otwierane w przeglądarce, aby zobaczyć, jak są one widoczne dla użytkownika.
> 
> Deweloperzy sieci Web używają zewnętrznych narzędzi do znajdowania problemów, które regularnie działają w przeglądarce. Następnie zwracają do środowiska IDE i zaczynają naprawianie. To działanie z powrotem i z tyłu między IDE, przeglądarką i narzędziami profilowania może być niewydajne i czasami wymaga nowego wdrożenia i oczyszczenia pamięci podręcznej za każdym razem, gdy chcesz odtworzyć problem.
> 
> Inspektor strony mostkuje przerwy w tworzeniu aplikacji sieci Web między klientem (narzędzia przeglądarki) i serwerem (ASP.NET i kod źródłowy), łącząc jednocześnie zalety obu środowisk przy użyciu połączonego zestawu funkcji.
> 
> Za pomocą narzędzia Page Inspector można zobaczyć, które elementy w plikach źródłowych (w tym kod po stronie serwera) wygenerowały znaczniki HTML, które mają być renderowane w przeglądarce. Narzędzie Page Inspector umożliwia również modyfikowanie właściwości CSS i atrybutów elementu DOM, aby zobaczyć zmiany odzwierciedlone bezpośrednio w przeglądarce.
> 
> W ramach tego praktycznego laboratorium przeprowadzisz użytkownika przez funkcje inspektora stron oraz pokazano, jak można ich używać do rozwiązywania problemów z aplikacjami sieci Web. **To laboratorium zawiera dwa ćwiczenia wykorzystujące podobne przepływy, ale ukierunkowane na różne technologie. Jeśli jesteś deweloperem ASP.NET MVC, wykonaj jedną z nich. Jeśli jesteś deweloperem w formie sieci Web, postępuj zgodnie z poniższymi instrukcjami**.
> 
> To laboratorium przeprowadzi Cię przez ulepszenia i nowe funkcje opisane wcześniej przez zastosowanie drobnych zmian do przykładowej aplikacji sieci Web, która znajduje się w folderze źródłowym.
> 
> Wszystkie przykładowe kod i fragmenty kodu są zawarte w zestawie szkoleń w sieci Web Camp, które są dostępne w [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym ćwiczeniu dowiesz się, jak:

- Rozłożyć witrynę internetową za pomocą narzędzia Page Inspector
- Sprawdzanie i Podgląd zmian stylów CSS za pomocą Inspektora stron
- Wykrywanie i rozwiązywanie problemów ze stron sieci Web za pomocą narzędzia Page Inspector

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

Aby ukończyć to laboratorium, musisz mieć następujące elementy:

- [Microsoft Visual Studio Express 2012 dla sieci Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub nadrzędnych (Przeczytaj [dodatek A,](#AppendixA) Aby uzyskać instrukcje na temat sposobu jego instalacji).
- Internet Explorer 9 lub nowszy

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Symulacyjn

To laboratorium praktyczne obejmuje następujące ćwiczenia:

1. [Ćwiczenie 1: korzystanie z narzędzia Page Inspector w projektach ASP.NET MVC](#Exercise1)
2. [Ćwiczenie 2: korzystanie z narzędzia Page Inspector w projektach formularzy WebForms](#Exercise2)

> [!NOTE]
> Każdemu z nich towarzyszy rozpoczęcie rozwiązanie znajdujące się w folderze startowym ćwiczenia, który umożliwia wykonywanie poszczególnych czynności niezależnie od innych. Wewnątrz kodu źródłowego dla ćwiczenia znajdziesz również folder końcowy zawierający rozwiązanie programu Visual Studio z kodem, który wynika z wykonania czynności w odpowiednim ćwiczeniu. Te rozwiązania można wykorzystać jako wskazówkę, jeśli potrzebujesz dodatkowej pomocy podczas pracy w ramach tego praktycznego laboratorium.

Szacowany czas wykonywania tego laboratorium: **30 minut**.

<a id="Exercise1"></a>

<a id="Exercise_1_Using_Page_Inspector_in_ASPNET_MVC_Projects"></a>
### <a name="exercise-1-using-page-inspector-in-aspnet-mvc-projects"></a>Ćwiczenie 1: korzystanie z narzędzia Page Inspector w projektach ASP.NET MVC

W tym ćwiczeniu dowiesz się, jak wyświetlać wersję zapoznawczą i debugować rozwiązanie **ASP.NET MVC 4** za pomocą narzędzia **Page Inspector**. Najpierw należy krótko wypróbować narzędzia, aby poznać funkcje, które ułatwiają proces debugowania w sieci Web. Następnie będziesz pracował na stronie sieci Web, która zawiera problemy z stylem. Dowiesz się, jak za pomocą Inspektora stron znaleźć kod źródłowy, który generuje problem, i napraw go.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>Zadanie 1 — Eksplorowanie inspektora stron

W tym zadaniu dowiesz się, jak używać inspektora stron w kontekście projektu ASP.NET MVC 4, który pokazuje galerię zdjęć.

1. Otwórz rozwiązanie **BEGIN** znajdujące się w lokalizacji **Source/Ex1-MVC4/BEGIN/** folder.

   1. Przed kontynuowaniem musisz pobrać brakujące pakiety NuGet. W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.
   2. W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu. Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu. To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.
2. W Eksplorator rozwiązań zlokalizuj widok **index. cshtml** w folderze projektu **/views/Home** , a następnie kliknij go prawym przyciskiem myszy, a następnie wybierz pozycję **Widok w Inspektorze strony**.

    ![Wybieranie pliku do podglądu w Inspektorze strony](using-page-inspector-in-visual-studio-2012/_static/image1.png "Wybieranie pliku do podglądu w Inspektorze strony")

    *Wybieranie pliku do podglądu w Inspektorze strony*
3. W oknie Inspektora stron zostanie wyświetlony adres URL */Home/index* mapowany do wybranego widoku źródła.

    ![Pierwszy kontakt z PageInspector](using-page-inspector-in-visual-studio-2012/_static/image2.png)

    *Pierwszy kontakt z inspektorem strony*

    Narzędzie Page Inspector jest zintegrowane w środowisku programu Visual Studio. Inspektor zawiera osadzoną przeglądarkę wraz z zaawansowanym profilerem HTML. Zwróć uwagę, że nie musisz uruchamiać rozwiązania, aby zobaczyć, jak wyglądają strony.

    > [!NOTE]
    > Gdy szerokość przeglądarki stron jest mniejsza niż szerokość otwartej strony, Strona nie zostanie prawidłowo wyświetlona. Jeśli tak się stanie, Dostosuj szerokość inspektora strony.
4. Kliknij kartę **pliki** w Inspektorze strony.

    Zobaczysz wszystkie pliki źródłowe, które tworzą stronę indeksu. Ta funkcja pomaga identyfikować wszystkie elementy w skrócie, szczególnie podczas pracy z częściowymi widokami i szablonami. Należy zauważyć, że po kliknięciu łączy można także otworzyć poszczególne pliki.

    ![Karta-Files](using-page-inspector-in-visual-studio-2012/_static/image3.png)

    *Karta pliki*
5. Kliknij przycisk **Przełącz tryb inspekcji** znajdujący się po lewej stronie kart.

    To narzędzie umożliwia wybranie dowolnego elementu strony i wyświetlenie jego kodu HTML i Razor.

    ![Toggle-Inspection-Mode-button](using-page-inspector-in-visual-studio-2012/_static/image4.png)

    *Przycisk przełączania trybu inspekcji*
6. W przeglądarce inspektora stron przesuń wskaźnik myszy nad elementy strony. Podczas przesuwania wskaźnika myszy nad dowolną część renderowanej strony, typ elementu jest wyświetlany, a odpowiednie znaczniki źródłowe lub kod są wyróżnione w edytorze programu Visual Studio.

    ![Tryb inspekcji w akcji](using-page-inspector-in-visual-studio-2012/_static/image5.png)

    *Tryb inspekcji w akcji*

    > [!NOTE]
    > Nie Maksymalizuj okna inspektora stron lub nie będzie można zobaczyć karty podglądu przedstawiającej kod źródłowy. Jeśli klikniesz element w Inspektorze strony, gdy zostanie on zmaksymalizowany, zostanie wyświetlony kod źródłowy zaznaczenia, ale spowoduje to ukrycie okna inspektora strony.

    Jeśli płacisz uwagę na plik **index. cshtml** , Zauważ, że fragment kodu źródłowego, który generuje wybrany element, jest wyróżniony. Ta funkcja ułatwia edytowanie długich plików źródłowych, zapewniając bezpośredni i szybki dostęp do kodu.

    ![Inspekcja elementów](using-page-inspector-in-visual-studio-2012/_static/image6.png)

    *Inspekcja elementów*
7. Kliknij przycisk **Przełącz tryb inspekcji** (![Wybierz kartę HTML, aby wyświetlić kod HTML renderowany w przeglądarce stron inspektora.](using-page-inspector-in-visual-studio-2012/_static/image7.png "Wybierz kartę HTML, aby wyświetlić kod HTML renderowany w przeglądarce inspektora stron.") ), aby wyłączyć kursor.
8. Wybierz kartę **HTML** , aby wyświetlić kod HTML renderowany w przeglądarce inspektora stron.
9. W znaczniku HTML Znajdź element listy za pomocą linku Koala i zaznacz go.

    Zauważ, że po wybraniu kodu odpowiednie dane wyjściowe zostaną automatycznie wyróżnione w przeglądarce. Ta funkcja jest przydatna do sprawdzenia, jak blok HTML jest renderowany na stronie.

    ![Wybieranie elementu HTML na stronie](using-page-inspector-in-visual-studio-2012/_static/image8.png "Wybieranie elementu HTML na stronie")

    *Wybieranie elementu HTML na stronie*
10. Kliknij przycisk **Przełącz tryb inspekcji** , aby włączyć *Tryb inspekcji* , a następnie kliknij pasek nawigacyjny. Po prawej stronie kodu HTML w okienku style zostanie wyświetlona lista ze stylami CSS zastosowanymi do wybranego elementu.

    > [!NOTE]
    > Ponieważ nagłówek jest częścią układu witryny, inspektor strony otworzy również plik \_Layout. cshtml i podświetli segment kodu, którego to dotyczy.

    ![Odnajdywanie stylów](using-page-inspector-in-visual-studio-2012/_static/image9.png)

    *Odnajdywanie stylów i plików źródłowych wybranego elementu*
11. Gdy jest włączony wskaźnik inspekcji przełączania, przesuń wskaźnik myszy poniżej niebieskiego paska polecanego i kliknij pół okręgu.

    ![Wybieranie elementu](using-page-inspector-in-visual-studio-2012/_static/image10.png "Wybieranie elementu")

    *Wybieranie elementu*
12. W okienku style Odszukaj element **tła obrazu** w grupie **. główna Zawartość** . **Wyczyść** **obraz tła** i zobacz, co się dzieje. Zobaczysz, że przeglądarka będzie odzwierciedlać zmiany natychmiast, a okrąg jest ukryty.

    > [!NOTE]
    > Zmiany stosowane na karcie style inspektora strony nie wpływają na oryginalny arkusz stylów. Możesz usunąć zaznaczenie stylów lub zmienić ich wartości dowolną liczbę razy, ale zostaną one przywrócone po odświeżeniu strony.

    ![Włączanie i wyłączanie stylów CSS](using-page-inspector-in-visual-studio-2012/_static/image11.png "Włączanie i wyłączanie stylów CSS")

    *Włączanie i wyłączanie stylów CSS*
13. Teraz kliknij tekst "**logo tutaj**" w nagłówku przy użyciu trybu inspekcji.
14. Na karcie **Style** odszukaj atrybut CSS **font-size** w grupie **titles** . Kliknij dwukrotnie wartość atrybutu i Zastąp wartość 2,3 em wartością **3 em**, a następnie naciśnij klawisz **Enter**. Zauważ, że tytuł jest większy.

    ![Zmiana wartości CSS w Inspektorze strony](using-page-inspector-in-visual-studio-2012/_static/image12.png "Zmiana wartości CSS w Inspektorze strony")

    *Zmiana wartości CSS w Inspektorze strony*
15. Kliknij kartę **Style śledzenia** znajdującą się w prawym okienku inspektora strony. Jest to alternatywny sposób wyświetlania wszystkich stylów zastosowanych do zaznaczenia, uporządkowane według nazwy atrybutu.

    ![Śledzenie stylów CSS](using-page-inspector-in-visual-studio-2012/_static/image13.png)

    *Śledzenie stylów CSS zaznaczonego elementu*
16. Inną funkcją inspektora stron jest okienko układu. Korzystając z trybu inspekcji, wybierz pasek nawigacyjny, a następnie kliknij kartę **Układ** w prawym okienku. Zobaczysz dokładny rozmiar wybranego elementu, a także jego przesunięcie, margines, wypełnienie i rozmiar obramowania. Należy zauważyć, że można także zmodyfikować wartości z tego widoku.

    ![Układ elementu w Inspektorze strony](using-page-inspector-in-visual-studio-2012/_static/image14.png "Układ elementu w Inspektorze strony")

    *Układ elementu w Inspektorze strony*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>Zadanie 2 — Wyszukiwanie i rozwiązywanie problemów z stylem w galerii zdjęć

Jak można zdiagnozować problemy ze stronami sieci Web przy użyciu poprzednich wersji programu Visual Studio? Prawdopodobnie znasz narzędzia debugowania sieci Web, które działają poza środowiskiem IDE programu Visual Studio, takimi jak Internet Explorer Narzędzia deweloperskie lub Firebug. Przeglądarki wiedzą tylko HTML, skrypty i style, natomiast podstawowa struktura generuje kod HTML, który będzie renderowany. Z tego powodu często trzeba wdrożyć całą lokację, aby zobaczyć, jak wyglądają strony sieci Web.

W przypadku, gdy chcesz wykryć i rozwiązać problem w witrynie sieci Web, prawdopodobnie zostały wykonane następujące czynności:

1. Uruchom rozwiązanie z programu Visual Studio lub Wdróż stronę na serwerze sieci Web.
2. W przeglądarce Otwórz narzędzia deweloperskie, których używasz, lub po prostu otwórz kod źródłowy i style i spróbuj dopasować problem. Aby znaleźć te pliki, można było używać&quot; wyszukiwania &quot;lub wyszukiwania &quot;w plikach&quot; funkcji z nazwą klas stylów.
3. Po wykryciu błędu Zatrzymaj przeglądarkę internetową i serwer.
4. Wyczyść pamięć podręczną przeglądarki.
5. Wróć do programu Visual Studio, aby zastosować poprawkę. Powtórz wszystkie kroki, które należy wykonać w celu przetestowania.

Ponieważ nie ma rzeczywistej wartości WYSIWYG w ASP.NET MVC 4, większość problemów z stylem jest wykrywanych na późniejszym etapie po uruchomieniu lub wdrożeniu aplikacji sieci Web. Teraz za pomocą narzędzia Page Inspector można wyświetlić podgląd dowolnej strony bez uruchamiania rozwiązania.

W tym zadaniu zostanie użyty Inspektor strony i rozwiązanie niektórych problemów z aplikacją Photo Gallery.

1. Za pomocą narzędzia Page Inspector Znajdź pozycję **Rejestr** i linki **Zaloguj** po lewej stronie nagłówka.

    Zauważ, że linki nie są wyświetlane w oczekiwanym miejscu po prawej stronie i są wyświetlane jako lista punktowana. Teraz będzie można wyrównać linki do prawej strony i odpowiednio zmienić ich styl.

    ![Lokalizowanie rejestru i linków logowania](using-page-inspector-in-visual-studio-2012/_static/image15.png "Lokalizowanie rejestru i linków logowania")

    *Lokalizowanie rejestru i linków logowania*
2. Po wybraniu trybu inspekcji Przełącz kliknij przycisk Zamknij do, ale nie na, a następnie zarejestruj łącze, aby otworzyć swój kod.

    Zwróć uwagę, że kod źródłowy linków znajduje się w pliku **\_LoginPartial. cshtml** , a nie index. cshtml ani \_Layout. cshtml, które są miejscami, które mogą się pojawić w pierwszej kolejności. Użytkownik został umieszczony bezpośrednio w poprawnym pliku źródłowym.
3. Na karcie **Style** zlokalizuj i kliknij **sekcję\<> #login** element, który jest kontenerem HTML dla tych linków.

    Zauważ, że styl **#login** jest automatycznie umieszczany w pliku **site. css** po kliknięciu. Ponadto kod jest teraz wyróżniony.

    ![Wybieranie stylów CSS](using-page-inspector-in-visual-studio-2012/_static/image16.png "Wybieranie stylów CSS")

    *Wybieranie stylów CSS*
4. Usuń komentarz do atrybutu **align tekstu** w wyróżnionym kodzie, usuwając znaki otwierający i zamykając i zapisując plik **site. css** .

    Inspektor strony ma świadomość wszystkich plików, które tworzą bieżącą stronę, i może wykryć, kiedy którykolwiek z tych plików się zmieni. Powiadamia użytkownika, gdy bieżąca strona w przeglądarce nie jest zsynchronizowana z plikami źródłowymi.
5. W przeglądarce inspektora stron kliknij pasek znajdujący się pod paskiem adresu, aby ponownie załadować stronę.

    ![Ponowne ładowanie strony](using-page-inspector-in-visual-studio-2012/_static/image17.png)

    *Ponowne ładowanie strony*

    Linki znajdują się teraz po prawej stronie, ale nadal wyglądają jak lista punktowana. Teraz można usunąć punktory i wyrównać linki w poziomie.

    ![Zaktualizowana Strona](using-page-inspector-in-visual-studio-2012/_static/image18.png)

    *Zaktualizowana Strona*
6. Korzystając z trybu inspekcji, zaznacz dowolne elementy **&lt;li&gt;** , które zawierają &quot;Register&quot; i &quot;Log in&quot;. Następnie kliknij **sekcję&lt;,&gt; #login** element, aby uzyskać dostęp do **stylów. css** Code.

    ![Wyszukiwanie stylu](using-page-inspector-in-visual-studio-2012/_static/image19.png "Wyszukiwanie stylu")

    *Wyszukiwanie stylu*
7. W **Style. css**, Usuń komentarz kodu dla elementów **#login li** . Dodawany styl spowoduje ukrycie punktora i wyświetlenie elementów w poziomie.

    ![Przetworzenie stylu linków logowania](using-page-inspector-in-visual-studio-2012/_static/image20.png "Przetworzenie stylu linków logowania")

    *Przetworzenie stylu linków logowania*
8. Zapisz plik **Style. css** i kliknij jeden raz na pasku znajdującym się pod adresem, aby ponownie załadować stronę. Zauważ, że linki są wyświetlane poprawnie.

    ![Linki wyrównane do prawej strony](using-page-inspector-in-visual-studio-2012/_static/image21.png "Linki wyrównane do prawej strony")

    *Linki wyrównane do prawej strony*
9. Na koniec zmienisz tytuł nagłówka. Użyj trybu inspekcji, aby kliknąć **tutaj logo** i przejść do kodu źródłowego, który go generuje.
10. Teraz jesteś w **\_Layout. cshtml.** Zastąp tekst "**logo tutaj**" "**galerią zdjęć**". Zapisz i Zaktualizuj przeglądarkę inspektora strony.

    ![Przypisywanie nowego tytułu](using-page-inspector-in-visual-studio-2012/_static/image22.png "Przypisywanie nowego tytułu")

    *Przypisywanie nowego tytułu*

    ![Strona galerii zdjęć](using-page-inspector-in-visual-studio-2012/_static/image23.png)

    *Strona galerii zdjęć została zaktualizowana*
11. Na koniec wybierz projekt **Galeria** i naciśnij klawisz **F5** , aby uruchomić aplikację. Sprawdź, czy wszystkie zmiany działają zgodnie z oczekiwaniami.

---

<a id="Exercise2"></a>

<a id="Exercise_2_Using_Page_Inspector_in_WebForms_Projects"></a>
### <a name="exercise-2-using-page-inspector-in-webforms-projects"></a>Ćwiczenie 2: korzystanie z narzędzia Page Inspector w projektach formularzy WebForms

W tym ćwiczeniu dowiesz się, jak wyświetlać i debugować rozwiązanie WebForms za pomocą narzędzia Page Inspector. Najpierw należy krótko uruchomić narzędzie, aby poznać funkcje inspektora stron, które ułatwiają proces debugowania sieci Web. Następnie będziesz pracował na stronie sieci Web, która zawiera problemy z stylem. Dowiesz się, jak za pomocą Inspektora stron znaleźć kod źródłowy, który generuje problem, i napraw go.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>Zadanie 1 — Eksplorowanie inspektora stron

W tym zadaniu dowiesz się, jak używać funkcji inspektora stron w kontekście projektu WebForms, który pokazuje galerię zdjęć.

1. Otwórz rozwiązanie **BEGIN** znajdujące się w lokalizacji **Source/Ex2-WebForms/BEGIN/** folder.

   1. Przed kontynuowaniem musisz pobrać brakujące pakiety NuGet. W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.
   2. W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu. Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu. To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.
2. Na Eksplorator rozwiązań odszukaj stronę **default. aspx** , kliknij ją prawym przyciskiem myszy, a następnie wybierz pozycję **Widok w Inspektorze strony**.

    ![Otwieranie default. aspx za pomocą Inspektora stron](using-page-inspector-in-visual-studio-2012/_static/image24.png "Otwieranie default. aspx za pomocą Inspektora stron")

    *Otwieranie default. aspx za pomocą Inspektora stron*
3. W oknie Inspektora stron zostanie wyświetlona wartość default. aspx.

    ![Wyświetlanie default. aspx w Inspektorze strony](using-page-inspector-in-visual-studio-2012/_static/image25.png "Wyświetlanie default. aspx w Inspektorze strony")

    *Wyświetlanie default. aspx w Inspektorze strony*

    Narzędzie Page Inspector jest zintegrowane w środowisku programu Visual Studio. Inspektor zawiera osadzoną przeglądarkę wraz z zaawansowanym profilerem HTML, który będzie pokazywał wybrany kod. Zwróć uwagę, że nie musisz uruchamiać rozwiązania, aby zobaczyć, jak wyglądają strony.

    > [!NOTE]
    > Gdy szerokość przeglądarki stron jest mniejsza niż szerokość otwartej strony, Strona nie zostanie prawidłowo wyświetlona. Jeśli tak się stanie, Dostosuj szerokość inspektora strony.
4. Kliknij kartę **pliki** w Inspektorze strony.

    Zobaczysz wszystkie pliki źródłowe, które tworzą renderowaną stronę domyślną. Jest to przydatna funkcja umożliwiająca szybkie zidentyfikowanie wszystkich elementów, szczególnie podczas pracy z kontrolkami użytkownika i stronami wzorcowymi. Zwróć uwagę, że możesz również przejść do każdego z plików.

    ![Karta pliki](using-page-inspector-in-visual-studio-2012/_static/image26.png "Karta pliki")

    *Karta pliki*
5. Kliknij przycisk **Przełącz tryb inspekcji** znajdujący się po lewej stronie kart.

    To narzędzie umożliwia wybranie dowolnego elementu strony i wyświetlenie jego kodu HTML i źródła. aspx.

    ![Przycisk przełączania trybu inspekcji](using-page-inspector-in-visual-studio-2012/_static/image27.png "Przycisk przełączania trybu inspekcji")

    *Przycisk przełączania trybu inspekcji*
6. W przeglądarce inspektora stron przesuń wskaźnik myszy na elementy strony. Podczas przesuwania wskaźnika myszy nad dowolną część renderowanej strony, typ elementu jest wyświetlany, a odpowiednie znaczniki źródłowe lub kod są wyróżnione w edytorze programu Visual Studio.

    ![Tryb inspekcji w akcji](using-page-inspector-in-visual-studio-2012/_static/image28.png "Tryb inspekcji w akcji")

    *Tryb inspekcji w akcji*

    > [!NOTE]
    > Nie Maksymalizuj okna inspektora stron lub nie będzie można zobaczyć karty podglądu przedstawiającej kod źródłowy. Jeśli klikniesz element w Inspektorze strony, gdy zostanie on zmaksymalizowany, zostanie wyświetlony kod źródłowy zaznaczenia, ale spowoduje to ukrycie okna inspektora strony.

    Jeśli płacisz uwagę na plik **default. aspx** , zobaczysz, że część kodu źródłowego, która generuje wybrany element, jest wyróżniona. Ta funkcja umożliwia obsługę wersji długich plików źródłowych, zapewniając bezpośredni i szybki dostęp do kodu.

    ![Inspekcja elementów](using-page-inspector-in-visual-studio-2012/_static/image29.png "Inspekcja elementów")

    *Inspekcja elementów*
7. Kliknij przycisk **Przełącz tryb inspekcji** (![Wybierz polecenie-HTML-Tab-to-Display-The-HTML-code--in](using-page-inspector-in-visual-studio-2012/_static/image30.png "Wybierz kolejno opcje-HTML-Tab-to-Display-The-HTML-code-Render-in-the-Page-Inspector-w przeglądarce.") --------------------------- ) znajdujący się na karcie inspektora strony, aby wyłączyć kursor.
8. Wybierz kartę **HTML** , aby wyświetlić kod HTML renderowany w przeglądarce inspektora stron.
9. W kodzie HTML Znajdź element listy za pomocą linku Koala i zaznacz go.

    Należy zauważyć, że po wybraniu kodu odpowiednie dane wyjściowe są automatycznie wyróżnione w przeglądarce. Ta funkcja jest przydatna do sprawdzenia, jak blok HTML jest renderowany na stronie.

    ![Wybieranie elementu HTML na stronie](using-page-inspector-in-visual-studio-2012/_static/image31.png "Wybieranie elementu HTML na stronie")

    *Wybieranie elementu HTML na stronie*
10. Kliknij przycisk **Przełącz tryb inspekcji** , aby włączyć *Tryb inspekcji* , a następnie kliknij pasek nawigacyjny. Po prawej stronie kodu HTML w okienku style zostanie wyświetlona lista ze stylami CSS zastosowanymi do wybranego elementu.

    > [!NOTE]
    > ponieważ nagłówek jest częścią układu witryny, inspektor strony otworzy również plik site. Master i podświetli segment kodu, którego to dotyczy.

    ![Odnajdywanie formularzy WebForms](using-page-inspector-in-visual-studio-2012/_static/image32.png "Odnajdywanie stylów i plików źródłowych wybranego elementu")

    *Odnajdywanie stylów i plików źródłowych wybranego elementu*
11. Gdy jest włączony wskaźnik inspekcji przełączania, przesuń wskaźnik myszy poniżej paska menu i kliknij puste pół okręgu.

    ![Wybieranie elementu](using-page-inspector-in-visual-studio-2012/_static/image33.png "Wybieranie elementu")

    *Wybieranie elementu*
12. W okienku style Odszukaj element **tła obrazu** w grupie **. główna Zawartość** . **Wyczyść** **obraz tła** i zobacz, co się dzieje. Zobaczysz, że przeglądarka będzie odzwierciedlać zmiany natychmiast, a okrąg jest ukryty.

    > [!NOTE]
    > Zmiany stosowane na karcie style inspektora strony nie wpływają na oryginalny arkusz stylów. Możesz usunąć zaznaczenie stylów lub zmienić ich wartości dowolną liczbę razy, ale zostaną one przywrócone po odświeżeniu strony.

    ![Włączanie i wyłączanie styles2 CSS](using-page-inspector-in-visual-studio-2012/_static/image34.png "Włączanie i wyłączanie stylów CSS")

    *Włączanie i wyłączanie stylów CSS*
13. Teraz kliknij**tekst "** **logo tutaj"** w nagłówku przy użyciu trybu inspekcji.
14. Na karcie **Style** odszukaj atrybut CSS **font-size** w grupie **titles** . Dwukrotnie kliknij atrybut raz, aby edytować jego wartość. Zastąp wartość 2.3 em wartością **3em**, a następnie naciśnij klawisz ENTER. Zauważ, że tytuł jest większy.

    ![Zmiana wartości CSS na stronie Inspector2](using-page-inspector-in-visual-studio-2012/_static/image35.png "Zmiana wartości CSS w Inspektorze strony")

    *Zmiana wartości CSS w Inspektorze strony*
15. Kliknij kartę **Style śledzenia** znajdującą się w prawym okienku inspektora strony. Jest to alternatywny sposób wyświetlania wszystkich stylów zastosowanych do zaznaczenia, uporządkowane według nazwy atrybutu.

    ![Śledzenie stylów CSS zaznaczonego elementu](using-page-inspector-in-visual-studio-2012/_static/image36.png "Śledzenie stylów CSS zaznaczonego elementu")

    *Śledzenie stylów CSS zaznaczonego elementu*
16. Inną funkcją inspektora stron jest okienko układu. Korzystając z trybu inspekcji, wybierz pasek nawigacyjny, a następnie kliknij kartę **Układ** w prawym okienku. Zobaczysz dokładny rozmiar wybranego elementu, a także jego przesunięcie, margines, wypełnienie i rozmiar obramowania. Należy zauważyć, że można także zmodyfikować wartości z tego widoku.

    ![Układ elementu w Inspektorze strony](using-page-inspector-in-visual-studio-2012/_static/image37.png "Układ elementu w Inspektorze strony")

    *Układ elementu w Inspektorze strony*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>Zadanie 2 — Wyszukiwanie i rozwiązywanie problemów z stylem w galerii zdjęć

Jak można zdiagnozować problemy ze stronami sieci Web przy użyciu poprzednich wersji programu Visual Studio? Prawdopodobnie znasz narzędzia debugowania sieci Web, które działają poza środowiskiem IDE programu Visual Studio, takimi jak Internet Explorer Narzędzia deweloperskie lub Firebug. Przeglądarki wiedzą tylko HTML, skrypty i style, natomiast podstawowa struktura generuje kod HTML, który będzie renderowany. Z tego powodu często trzeba wdrożyć całą lokację, aby zobaczyć, jak wyglądają strony sieci Web.

W przypadku, gdy chcesz wykryć i rozwiązać problem w witrynie sieci Web, prawdopodobnie zostały wykonane następujące czynności:

1. Uruchom rozwiązanie z programu Visual Studio lub Wdróż stronę na serwerze sieci Web.
2. W przeglądarce Otwórz narzędzia deweloperskie, których używasz, lub po prostu otwórz kod źródłowy i style i spróbuj dopasować problem. Aby znaleźć te pliki, użyto &quot;wyszukiwania&quot; lub &quot;wyszukiwanie w plikach&quot; funkcji z nazwą klas stylu.
3. Po wykryciu błędu Zatrzymaj przeglądarkę internetową i serwer.
4. Wyczyść pamięć podręczną przeglądarki.
5. Wróć do programu Visual Studio, aby zastosować poprawkę. Powtórz wszystkie kroki, które należy wykonać w celu przetestowania.

Ponieważ nie ma rzeczywistych wartości WYSIWYG w ASP.NET WebForms, niektóre problemy z stylem są wykrywane na późniejszym etapie po uruchomieniu lub wdrożeniu. Teraz za pomocą narzędzia Page Inspector można wyświetlić podgląd dowolnej strony bez uruchamiania rozwiązania.

W tym zadaniu zostanie użyty Inspektor strony służący do rozwiązywania niektórych problemów z aplikacją Photo Gallery. W poniższych krokach wykryjesz i szybko naprawisz prosty problem z stylem w nagłówku.

1. Za pomocą funkcji inspekcja stron Znajdź pozycję **Rejestr** i linki **Zaloguj** po lewej stronie nagłówka.

    Zwróć uwagę, że link nie jest wyświetlany w oczekiwanym miejscu po prawej stronie. Teraz możesz wyrównać link do prawej strony i odpowiednio zmienić jego styl.

    ![Link logowania umieszczony po lewej stronie](using-page-inspector-in-visual-studio-2012/_static/image38.png "Link logowania umieszczony po lewej stronie")

    *Link logowania umieszczony po lewej stronie*
2. Po wybraniu opcji Przełącz tryb inspekcji wybierz link Zaloguj się, aby otworzyć swój kod.

    Zwróć uwagę, że kod źródłowy linku znajduje się w pliku **site. Master** , a nie na stronie Default. aspx, która jest miejscem, w którym można się zajrzeć. Użytkownik został umieszczony bezpośrednio w poprawnym pliku źródłowym.
3. Na karcie **Style** zlokalizuj i kliknij **sekcję&lt;&gt; #login** element, który jest kontenerem HTML dla tych linków.

    Zauważ, że styl **#login** jest automatycznie umieszczany w pliku **site. css** po kliknięciu. Ponadto kod jest teraz wyróżniony.

    ![Wybieranie stylów CSS](using-page-inspector-in-visual-studio-2012/_static/image39.png "Wybieranie stylów CSS")

    *Wybieranie stylów CSS*
4. Usuń komentarz do atrybutu **align tekstu** w wyróżnionym kodzie, usuwając znaki otwierający i zamykając i zapisując plik **site. css** .

    Inspektor strony ma świadomość wszystkich plików, które tworzą bieżącą stronę, i może wykryć, kiedy którykolwiek z tych plików się zmieni. Powiadamia użytkownika, gdy bieżąca strona w przeglądarce nie jest zsynchronizowana z plikami źródłowymi.
5. W przeglądarce inspektora stron kliknij pasek znajdujący się pod paskiem adresu, aby zapisać zmiany, a następnie ponownie Załaduj stronę.

    ![Ponowne ładowanie strony](using-page-inspector-in-visual-studio-2012/_static/image40.png)

    *Ponowne ładowanie strony*

    Linki znajdują się teraz po prawej stronie, ale nadal wyglądają jak lista punktowana. Teraz można usunąć punktory i wyrównać linki w poziomie.

    ![Zaktualizowana Strona](using-page-inspector-in-visual-studio-2012/_static/image41.png)

    *Zaktualizowana Strona*
6. Korzystając z trybu inspekcji, zaznacz dowolne elementy **&lt;li&gt;** , które zawierają &quot;Register&quot; i &quot;Log in&quot;. Następnie kliknij **sekcję&lt;,&gt; #login** element, aby uzyskać dostęp do **stylów. css** Code.

    ![Wyszukiwanie stylu](using-page-inspector-in-visual-studio-2012/_static/image42.png "Wyszukiwanie stylu")

    *Wyszukiwanie stylu*
7. W **Style. css**, Usuń komentarz kodu dla elementów **#login li** . Dodawany styl spowoduje ukrycie punktora i wyświetlenie elementów w poziomie.

    ![Przetworzenie stylu linków logowania](using-page-inspector-in-visual-studio-2012/_static/image43.png "Przetworzenie stylu linków logowania")

    *Przetworzenie stylu linków logowania*
8. Zapisz plik **Style. css** i kliknij jeden raz na pasku znajdującym się pod adresem, aby ponownie załadować stronę. Zauważ, że linki są wyświetlane poprawnie.

    ![Linki wyrównane do prawej strony](using-page-inspector-in-visual-studio-2012/_static/image44.png "Linki wyrównane do prawej strony")

    *Linki wyrównane do prawej strony*
9. Na koniec zmienisz tytuł nagłówka. Zamiast wyszukać tekst "**logo tutaj"** we wszystkich plikach, użyj trybu inspekcji, aby kliknąć tekst i przejść do kodu źródłowego, który go generuje.

    ![Znajdowanie tytułu witryny](using-page-inspector-in-visual-studio-2012/_static/image45.png "Znajdowanie tytułu witryny")

    *Znajdowanie tytułu witryny*
10. Teraz jesteś w **witrynie site. Master.** Zastąp tekst "**logo tutaj**" "**galerią zdjęć**". Zapisz i Zaktualizuj przeglądarkę inspektora strony.

    ![Strona galerii zdjęć została zaktualizowana](using-page-inspector-in-visual-studio-2012/_static/image46.png "Strona galerii zdjęć została zaktualizowana")

    *Strona galerii zdjęć została zaktualizowana*
11. Na koniec naciśnij klawisz **F5** , aby uruchomić aplikację. Sprawdź, czy wszystkie zmiany działają zgodnie z oczekiwaniami.

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

Wykonując te praktyczne ćwiczenia, dowiesz się, jak za pomocą Inspektora stron wyświetlić podgląd aplikacji sieci Web bez konieczności ponownego kompilowania i uruchamiania witryny sieci Web w przeglądarce. Ponadto dowiesz się, jak szybko znajdować i naprawiać usterki, uzyskując dostęp bezpośrednio z renderowanych danych wyjściowych do kodu źródłowego.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Dodatek A: Instalowanie Visual Studio Express 2012 dla sieci Web

Można zainstalować **Microsoft Visual Studio Express 2012 dla sieci Web** lub innej &quot;Express&quot; wersji za pomocą **[Instalator platformy Microsoft Web](https://www.microsoft.com/web/downloads/platform.aspx)** . Poniższe instrukcje przeprowadzą Cię przez kroki wymagane do zainstalowania *programu Visual Studio Express 2012 for Web* przy użyciu *Instalator platformy Microsoft Web*.

1. Przejdź do [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Alternatywnie, jeśli zainstalowano już Instalatora platformy sieci Web, można go otworzyć i wyszukać produkt &quot;<em>Visual Studio Express 2012 for Web przy użyciu zestawu Windows Azure SDK</em>&quot;.
2. Kliknij pozycję **Zainstaluj teraz**. Jeśli nie masz **Instalatora platformy sieci Web** , nastąpi przekierowanie do pobrania i zainstalowania go w pierwszej kolejności.
3. Po otwarciu **Instalatora platformy sieci Web** kliknij przycisk **Zainstaluj** , aby rozpocząć instalację.

    ![Zainstaluj Visual Studio Express](using-page-inspector-in-visual-studio-2012/_static/image47.png "Zainstaluj Visual Studio Express")

    *Zainstaluj Visual Studio Express*
4. Przeczytaj wszystkie licencje i postanowienia produktów, a następnie kliknij przycisk **Akceptuję** , aby kontynuować.

    ![Akceptowanie postanowień licencyjnych](using-page-inspector-in-visual-studio-2012/_static/image48.png)

    *Akceptowanie postanowień licencyjnych*
5. Poczekaj na zakończenie procesu pobierania i instalacji.

    ![Postęp instalacji](using-page-inspector-in-visual-studio-2012/_static/image49.png)

    *Postęp instalacji*
6. Po zakończeniu instalacji kliknij przycisk **Zakończ**.

    ![Instalacja zakończona](using-page-inspector-in-visual-studio-2012/_static/image50.png)

    *Instalacja zakończona*
7. Kliknij przycisk **Zakończ** , aby zamknąć Instalatora platformy sieci Web.
8. Aby otworzyć Visual Studio Express dla sieci Web, przejdź do ekranu **startowego** i zacznij pisać &quot;**vs Express**&quot;, a następnie kliknij kafelek **vs Express for Web** .

    ![Kafelek VS Express for Web](using-page-inspector-in-visual-studio-2012/_static/image51.png)

    *Kafelek VS Express for Web*

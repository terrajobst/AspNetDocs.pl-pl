---
uid: web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-vb
title: Zagnieżdżone strony wzorcowe (VB) | Microsoft Docs
author: rick-anderson
description: Pokazuje, jak zagnieżdżać jedną stronę wzorcową w innej.
ms.author: riande
ms.date: 07/28/2008
ms.assetid: 14d9aa1b-4dca-43a0-aa9d-a6e891fee019
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 9bb39712855c37f5cbcbb447f7691e9451b8dc92
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74642001"
---
# <a name="nested-master-pages-vb"></a>Zagnieżdżone strony wzorcowe (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_VB.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_VB.pdf)

> Pokazuje, jak zagnieżdżać jedną stronę wzorcową w innej.

## <a name="introduction"></a>Wprowadzenie

W ciągu ostatnich dziewięciu samouczków zaobserwowano sposób implementacji układu dla całej witryny za pomocą stron wzorcowych. W Nutshell strony wzorcowe pozwalają nam, deweloperowi strony definiować typowe znaczniki na stronie wzorcowej wraz z określonymi regionami, które można dostosować na podstawie strony zawartości na stronie zawartość. Kontrolki ContentPlaceHolder na stronie wzorcowej wskazują edytowalne regiony; niestandardowe znaczniki formantów ContentPlaceHolder są zdefiniowane na stronie zawartości za pomocą kontrolek zawartości.

Techniki na stronie wzorcowej, które zostały dotąd omówione, są wspaniałe, jeśli masz jeden układ w całej lokacji. Jednak wiele dużych witryn sieci Web ma układ witryny dostosowany do różnych sekcji. Rozważmy na przykład aplikację opieki zdrowotnej używaną przez personel szpitaly do zarządzania informacjami, działaniami i rozliczeniami pacjenta. W tej aplikacji mogą istnieć trzy typy stron sieci Web:

- Strony dla pracowników, w których członkowie personelu mogą aktualizować dostępność, wyświetlać harmonogramy lub żądać czasu urlopu.
- Strony specyficzne dla pacjenta, w których członkowie personelu wyświetlają lub edytują informacje dla określonego pacjenta.
- Strony dotyczące rozliczeń, w których Księgowość przegląda bieżące Stany roszczeń i raporty finansowe.

Każda strona może korzystać ze wspólnego układu, takiego jak menu w górnej części i seria często używanych linków w dolnej części ekranu. Jednak strony dotyczące personelu, pacjenta i rozliczeń mogą wymagać dostosowania tego Układu ogólnego. Na przykład, być może wszystkie strony specyficzne dla personelu powinny zawierać kalendarz i listę zadań, które pokazują dostępność aktualnie zalogowanego użytkownika i dzienny harmonogram. Być może wszystkie strony specyficzne dla pacjenta muszą wyświetlić nazwę, adres i informacje o ubezpieczeniach dla pacjenta, których informacje są edytowane.

Istnieje możliwość utworzenia takich dostosowanych układów przy użyciu *zagnieżdżonych stron wzorcowych*. Aby zaimplementować powyższy scenariusz, możemy zacząć od utworzenia strony wzorcowej, która definiuje układ całej witryny, zawartość menu i stopki przy użyciu elementów ContentPlaceHolder definiujących edytowalne regiony. Następnie utworzymy trzy zagnieżdżone strony wzorcowe, jeden dla każdego typu strony sieci Web. Każda zagnieżdżona Strona wzorcowa będzie definiować zawartość między typem stron zawartości, które korzystają z strony wzorcowej. Inaczej mówiąc, zagnieżdżona Strona wzorcowa dla stron zawartości z konkretnym pacjentem będzie zawierać znaczniki i logikę programistyczną służącą do wyświetlania informacji o edytowanym pacjentu. Podczas tworzenia nowej strony specyficznej dla pacjenta można powiązać ją z tą zagnieżdżoną stroną wzorcową.

Ten samouczek zaczyna się od wyróżniania korzyści zagnieżdżonych stron wzorcowych. Następnie pokazano, jak tworzyć i używać zagnieżdżonych stron wzorcowych.

> [!NOTE]
> Zagnieżdżone strony wzorcowe były możliwe od wersji 2,0 .NET Framework. Jednak program Visual Studio 2005 nie zawiera obsługi czasu projektowania dla zagnieżdżonych stron wzorcowych. Dobra wiadomość polega na tym, że program Visual Studio 2008 oferuje bogate środowisko czasu projektowania dla zagnieżdżonych stron wzorcowych. Jeśli interesuje Cię korzystanie z zagnieżdżonych stron głównych, ale nadal korzystasz z programu Visual Studio 2005, zapoznaj się z wpisem w blogu [Scott Guthrie](https://weblogs.asp.net/scottgu/), [porady dotyczące zagnieżdżonych stron wzorcowych w czasie projektowania programu VS 2005](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx).

## <a name="the-benefits-of-nested-master-pages"></a>Zalety zagnieżdżonych stron wzorcowych

Wiele witryn sieci Web ma przebudowany projekt witryny, a także bardziej dostosowane projekty charakterystyczne dla niektórych typów stron. Na przykład w naszej demonstracyjnej aplikacji sieci Web została utworzona sekcja administracji podstawowe (strony w folderze `~/Admin`). Obecnie strony sieci Web w folderze `~/Admin` używają tej samej strony wzorcowej co te strony, a nie w sekcji Administracja (tj. `Site.master` lub `Alternate.master`, w zależności od wyboru użytkownika).

> [!NOTE]
> Na razie poudawać, że witryna ma tylko jedną stronę wzorcową, `Site.master`. Będziemy używać zagnieżdżonych stron wzorcowych z dwiema (lub więcej) stronami wzorcowymi, zaczynając od "użycie zagnieżdżonej strony wzorcowej dla sekcji administracyjnej" w dalszej części tego samouczka.

Załóżmy, że został poproszony o dostosowanie układu stron administracyjnych w celu uwzględnienia dodatkowych informacji lub linków, które w przeciwnym razie nie będą obecne na innych stronach w witrynie. Istnieją cztery techniki implementowania tego wymagania:

1. Ręcznie Dodaj informacje dotyczące administracji i linki do każdej strony zawartości w folderze `~/Admin`.
2. Zaktualizuj stronę wzorcową `Site.master`, aby zawierała informacje i linki dotyczące sekcji administracji, a następnie Dodaj kod do strony głównej, aby pokazać lub ukryć te sekcje w zależności od tego, czy jedna ze stron administracyjnych jest odwiedzana.
3. Utwórz nową stronę wzorcową dla sekcji Administracja, skopiuj ją do znacznika z `Site.master`, Dodaj informacje i linki dotyczące sekcji Administracja, a następnie zaktualizuj strony zawartości w folderze `~/Admin`, aby użyć tej nowej strony wzorcowej.
4. Utwórz zagnieżdżoną stronę wzorcową, która wiąże się z `Site.master` i że strony zawartości w folderze `~/Admin` używają tej nowej zagnieżdżonej strony wzorcowej. Ta zagnieżdżona Strona wzorcowa będzie zawierać tylko dodatkowe informacje i linki specyficzne dla stron administracyjnych i nie musi powtarzać znaczników już zdefiniowanych w `Site.master`.

Pierwsza opcja to najmniejszy palatable. Cały punkt korzystania ze stron wzorcowych polega na tym, że trzeba ręcznie kopiować i wklejać typowe znaczniki do nowych stron ASP.NET. Druga opcja jest akceptowalna, ale sprawia, że aplikacja jest mniej utrzymywana, gdy tworzy zbiorczo strony wzorcowe ze znacznikiem, które jest wyświetlane tylko sporadycznie, i wymaga, aby deweloperzy edytujący stronę wzorcową mogli obejść tę adiustację i zapamiętać, że dokładnie niektóre znaczniki są wyświetlane w przeciwieństwie do ukrycia. Takie podejście byłoby mniej Tenable, ponieważ dostosowania z więcej niż większej liczby typów stron sieci Web, które trzeba uwzględnić na tej pojedynczej stronie wzorcowej.

Trzecia opcja eliminuje problemy z bałaganem i złożonością wyznaczoną dla drugiej opcji. Jednak w przypadku opcji trzech głównej wadą jest wymagane skopiowanie i wklejenie wspólnego układu z `Site.master` do nowej strony głównej konkretnej sekcji administracji. W przypadku późniejszej decyzji o zmianie układu dla całej witryny należy pamiętać, aby zmienić go w dwóch miejscach.

Czwarta opcja zagnieżdżonych stron wzorcowych daje nam najlepsze z drugiej i trzeciej opcji. Informacje o układzie dla całej witryny są przechowywane w jednym pliku — stronie wzorcowej najwyższego poziomu — w przypadku oddzielenia zawartości specyficznej dla określonych regionów na różne pliki.

Ten samouczek rozpoczyna się od utworzenia i użycia prostej zagnieżdżonej strony wzorcowej. Tworzymy nową stronę wzorcową najwyższego poziomu, dwie zagnieżdżone strony wzorcowe i dwie strony zawartości. Począwszy od opcji "Używanie zagnieżdżonej strony wzorcowej dla sekcji Administracja" Przyjrzyjmy się aktualizowaniu istniejącej architektury strony głównej, aby uwzględnić użycie zagnieżdżonych stron wzorcowych. W szczególnych przypadkach tworzymy zagnieżdżoną stronę wzorcową i użyjemy jej do dołączania dodatkowej zawartości niestandardowej do stron zawartości w folderze `~/Admin`.

## <a name="step-1-creating-a-simple-top-level-master-page"></a>Krok 1. tworzenie prostej strony wzorcowej najwyższego poziomu

Utworzenie zagnieżdżonego wzorca opartego na jednej z istniejących stron wzorcowych, a następnie zaktualizowanie istniejącej strony zawartości do korzystania z tej nowej zagnieżdżonej strony wzorcowej zamiast strony wzorcowej najwyższego poziomu wiąże się z pewnymi złożonością, ponieważ istniejące strony zawartości już oczekują na niektóre Kontrolki ContentPlaceHolder zdefiniowane na stronie wzorcowej najwyższego poziomu. W związku z tym zagnieżdżona Strona wzorcowa musi również zawierać te same kontrolki ContentPlaceHolder o tych samych nazwach. Ponadto nasza aplikacja demonstracyjna ma dwie strony wzorcowe (`Site.master` i `Alternate.master`), które są dynamicznie przypisywane do strony zawartości w oparciu o preferencje użytkownika, co dodatkowo dodaje do tej złożoności. Zapoznajemy się z aktualizacją istniejącej aplikacji w celu używania zagnieżdżonych stron wzorcowych w dalszej części tego samouczka, ale najpierw skupmy się na prostym przykładzie zagnieżdżonych stron wzorcowych.

Utwórz nowy folder o nazwie `NestedMasterPages`, a następnie Dodaj nowy plik strony głównej do tego folderu o nazwie `Simple.master`. (Zobacz rysunek 1 zrzut ekranu przedstawiający Eksplorator rozwiązań po dodaniu tego folderu i pliku). Przeciągnij plik arkusza stylów `AlternateStyles.css` z Eksplorator rozwiązań do projektanta. Spowoduje to dodanie elementu `<link>` do pliku arkusza stylów w elemencie `<head>`, po którym będzie wyglądać znacznik elementu `<head>` strony wzorcowej:

[!code-aspx[Main](nested-master-pages-vb/samples/sample1.aspx)]

Następnie Dodaj następujący znacznik w formie sieci Web `Simple.master`:

[!code-aspx[Main](nested-master-pages-vb/samples/sample2.aspx)]

Ten znacznik wyświetla link zatytułowany "zagnieżdżone strony wzorcowe (proste)" w górnej części strony w dużej białej czcionce na granatowym tle. Poniżej znajduje się `MainContent` ContentPlaceHolder. Rysunek 1 przedstawia stronę wzorcową `Simple.master` po załadowaniu jej w programie Visual Studio Designer.

[![zagnieżdżona Strona wzorcowa definiuje zawartość specyficzną dla stron w sekcji Administracja.](nested-master-pages-vb/_static/image2.png)](nested-master-pages-vb/_static/image1.png)

**Ilustracja 01**. zagnieżdżona Strona wzorcowa definiuje zawartość specyficzną dla stron w sekcji Administracja ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](nested-master-pages-vb/_static/image3.png))

## <a name="step-2-creating-a-simple-nested-master-page"></a>Krok 2. tworzenie prostej zagnieżdżonej strony wzorcowej

`Simple.master` zawiera dwie kontrolki ContentPlaceHolder: element `MainContent` ContentPlaceHolder, który został dodany w formularzu sieci Web wraz z `head` ContentPlaceHolder w elemencie `<head>`. Jeśli udało nam się utworzyć stronę zawartości i powiązać ją z `Simple.master` strona zawartości będzie miała dwie kontrolki zawartości odwołujące się do dwóch elementów ContentPlaceHolder. Podobnie, jeśli utworzymy zagnieżdżoną stronę wzorcową i powiążesz ją z `Simple.master`, zagnieżdżona Strona wzorcowa będzie miała dwie kontrolki zawartości.

Dodajmy nową zagnieżdżoną stronę wzorcową do folderu `NestedMasterPages` o nazwie `SimpleNested.master`. Kliknij prawym przyciskiem myszy folder `NestedMasterPages` i wybierz polecenie Dodaj nowy element. Spowoduje to wyświetlenie okna dialogowego Dodawanie nowego elementu pokazanego na rysunku 2. Wybierz typ szablonu strony głównej i wpisz nazwę nowej strony głównej. Aby wskazać, że nowa strona wzorcowa powinna być zagnieżdżoną stroną wzorcową, zaznacz pole wyboru "Wybierz stronę wzorcową".

Następnie kliknij przycisk Dodaj. Spowoduje to wyświetlenie okna dialogowego Wybierz stronę wzorcową, które zostanie wyświetlone podczas wiązania strony zawartości ze stroną wzorcową (patrz rysunek 3). Wybierz stronę wzorcową `Simple.master` w folderze `NestedMasterPages` i kliknij przycisk OK.

> [!NOTE]
> Jeśli witryna sieci Web ASP.NET została utworzona przy użyciu modelu projektu aplikacji internetowej zamiast modelu projektu witryny sieci Web, w oknie dialogowym Dodaj nowy element zostanie wyświetlona wartość pola wyboru "Wybierz stronę wzorcową" na rysunku 2. Aby utworzyć zagnieżdżoną stronę wzorcową podczas korzystania z modelu projektu aplikacji sieci Web, należy wybrać zagnieżdżony szablon strony wzorcowej (zamiast szablonu strony głównej). Po wybraniu szablonu zagnieżdżonej strony wzorcowej i kliknięciu przycisku Dodaj zostanie wyświetlona ta sama okno dialogowe Wybieranie strony głównej pokazane na rysunku 3.

[![zaznacz pole wyboru &quot;wybierz stronę wzorcową&quot;, aby dodać zagnieżdżoną stronę wzorcową](nested-master-pages-vb/_static/image5.png)](nested-master-pages-vb/_static/image4.png)

**Ilustracja 02**. Zaznacz pole wyboru "Wybierz stronę wzorcową", aby dodać zagnieżdżoną stronę wzorcową ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](nested-master-pages-vb/_static/image6.png))

[![powiązać zagnieżdżoną stronę wzorcową z prostą. wzorcową stroną wzorcową](nested-master-pages-vb/_static/image8.png)](nested-master-pages-vb/_static/image7.png)

**Rysunek 03**: Powiąż zagnieżdżoną stronę wzorcową ze stroną wzorcową `Simple.master` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](nested-master-pages-vb/_static/image9.png))

Rozszerzalne znaczniki strony wzorcowej, pokazane poniżej, zawierają dwie kontrolki zawartości odwołujące się do dwóch formantów ContentPlaceHolder na stronie wzorca najwyższego poziomu.

[!code-aspx[Main](nested-master-pages-vb/samples/sample3.aspx)]

Z wyjątkiem dyrektywy `<%@ Master %>` pierwotna deklaracyjnej strony wzorcowej jest taka sama jak znacznik, który jest początkowo generowany podczas wiązania strony zawartości z tą samą stroną wzorcową najwyższego poziomu. Podobnie jak w przypadku dyrektywy `<%@ Page %>` strony zawartości, dyrektywa `<%@ Master %>` zawiera atrybut `MasterPageFile`, który określa nadrzędną stronę wzorcową strony wzorcowej. Główną różnicą między zagnieżdżoną stroną wzorcową a stroną zawartości powiązaną z tą samą stroną wzorcową najwyższego poziomu jest to, że zagnieżdżona Strona wzorcowa może zawierać kontrolki ContentPlaceHolder. Kontrolki ContentPlaceHolder zagnieżdżonej strony wzorcowej definiują regiony, w których strony zawartości mogą dostosowywać adiustację.

Zaktualizuj tę zagnieżdżoną stronę wzorcową tak, aby wyświetlała tekst "Hello, from SimpleNested!" w kontrolce zawartości, która odnosi się do formantu `MainContent` ContentPlaceHolder.

[!code-aspx[Main](nested-master-pages-vb/samples/sample4.aspx)]

Po dokonaniu tego dodawania Zapisz zagnieżdżoną stronę wzorcową, a następnie Dodaj nową stronę zawartości do folderu `NestedMasterPages` o nazwie `Default.aspx`i powiąż go ze stroną wzorcową `SimpleNested.master`. Po dodaniu tej strony może być niedostępna, aby zobaczyć, że nie zawiera żadnych kontrolek zawartości (zobacz rysunek 4). Strona zawartości może uzyskać dostęp tylko do elementów ContentPlaceHolder swojej *nadrzędnej* strony wzorcowej. `SimpleNested.master` nie zawiera żadnych kontrolek ContentPlaceHolder; w związku z tym każda strona zawartości związana z tą stroną wzorcową nie może zawierać żadnych kontrolek zawartości.

[![Nowa strona zawartości nie zawiera żadnych kontrolek zawartości](nested-master-pages-vb/_static/image11.png)](nested-master-pages-vb/_static/image10.png)

**Ilustracja 04**: Nowa strona zawartości nie zawiera żadnych kontrolek zawartości ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](nested-master-pages-vb/_static/image12.png))

Należy zaktualizować zagnieżdżoną stronę wzorcową (`SimpleNested.master`), aby zawierała kontrolki ContentPlaceHolder. Zazwyczaj można chcieć, aby zagnieżdżone strony wzorcowe zawierały element ContentPlaceHolder dla każdego elementu ContentPlaceHolder zdefiniowanego przez jego nadrzędną stronę wzorcową, dzięki czemu jego podrzędna Strona wzorcowa lub strona zawartości może współdziałać z dowolną stroną wzorcową najwyższego poziomu. kontrolek.

Zaktualizuj stronę wzorcową `SimpleNested.master`, aby zawierała element ContentPlaceHolder w swoich dwóch kontrolkach zawartości. Nadaj formantowi ContentPlaceHolder taką samą nazwę, jak formant ContentPlaceHolder, do którego odwołuje się kontrolka zawartości. Oznacza to, że Dodaj kontrolkę ContentPlaceHolder o nazwie `MainContent` do kontrolki zawartość w `SimpleNested.master`, która odwołuje się do `MainContent` ContentPlaceHolder w `Simple.master`. Wykonaj tę samą czynność w kontrolce zawartości, która odwołuje się do `head` ContentPlaceHolder.

> [!NOTE]
> Chociaż zaleca się nazywanie formantów ContentPlaceHolder na zagnieżdżonej stronie wzorcowej tak samo jak elementy ContentPlaceHolder na stronie wzorcowej najwyższego poziomu, ta symetria nazewnictwa nie jest wymagana. Możesz nadać kontrolki ContentPlaceHolder na zagnieżdżonej stronie wzorcowej dowolną nazwę. Można jednak łatwiej zapamiętać, jakie elementy ContentPlaceHolder odpowiadają regionom strony, jeśli na stronie wzorcowej najwyższego poziomu i zagnieżdżonych stronach wzorcowych są używane te same nazwy.

Po wprowadzeniu tych uzupełnień deklaratywne znaczniki strony wzorcowej `SimpleNested.master` powinny wyglądać podobnie do następujących:

[!code-aspx[Main](nested-master-pages-vb/samples/sample5.aspx)]

Usuń właśnie utworzoną stronę zawartości `Default.aspx`, a następnie dodaj ją ponownie, aby powiązać ją ze stroną wzorcową `SimpleNested.master`. Tym razem program Visual Studio dodaje do `Default.aspx`dwie kontrolki zawartości, odwołujące się do elementów ContentPlaceHolder teraz zdefiniowanych w `SimpleNested.master` (zobacz rysunek 6). Dodaj tekst "Hello, z default. aspx!" w kontrolce zawartości, do której odwołuje się `MainContent`.

Rysunek 5 przedstawia trzy jednostki występujące w tym miejscu — `Simple.master`, `SimpleNested.master`i `Default.aspx` oraz sposób ich powiązania ze sobą. Jak widać na diagramie, zagnieżdżona Strona wzorcowa implementuje formanty zawartości dla elementu ContentPlaceHolder. Jeśli te regiony muszą być dostępne dla strony zawartości, zagnieżdżona Strona wzorcowa musi dodać własne elementy ContentPlaceHolders do kontrolek zawartości.

[![na najwyższego poziomu i zagnieżdżonych stronach wzorcowych dyktowanie układu strony zawartości](nested-master-pages-vb/_static/image14.png)](nested-master-pages-vb/_static/image13.png)

**Ilustracja 05**: strony nadrzędne najwyższego poziomu i zagnieżdżonych stron wzorcowych wymuszają układ strony zawartości ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](nested-master-pages-vb/_static/image15.png))

To zachowanie ilustruje, jak strona zawartości lub strona wzorcowa jest firma Cognizant tylko nadrzędnej strony głównej. To zachowanie jest również wskazywane przez projektanta programu Visual Studio. Rysunek 6 przedstawia projektanta `Default.aspx`. Gdy projektant jasno pokazuje, które regiony są edytowalne ze strony zawartości i jakie fragmenty nie są, nie można odróżnić regionów, które nie są edytowalne, są z zagnieżdżonej strony wzorcowej i jakie regiony znajdują się na stronie wzorcowej najwyższego poziomu.

[![strona zawartości zawiera teraz kontrolki zawartości dla elementów ContentPlaceHolder zagnieżdżonej strony wzorcowej](nested-master-pages-vb/_static/image17.png)](nested-master-pages-vb/_static/image16.png)

**Ilustracja 06**. Strona zawartości zawiera teraz formanty zawartości dla elementów ContentPlaceHolder zagnieżdżonej strony wzorcowej ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](nested-master-pages-vb/_static/image18.png))

## <a name="step-3-adding-a-second-simple-nested-master-page"></a>Krok 3. Dodawanie drugiej prostej zagnieżdżonej strony wzorcowej

Korzystanie z zagnieżdżonych stron wzorcowych jest bardziej oczywiste, gdy istnieje wiele zagnieżdżonych stron wzorcowych. Aby zilustrować tę korzyść, należy utworzyć kolejną zagnieżdżoną stronę wzorcową w folderze `NestedMasterPages`. Nazwij tę nową zagnieżdżoną stronę wzorcową `SimpleNestedAlternate.master` i powiąż ją ze stroną wzorcową `Simple.master`. Dodaj kontrolki ContentPlaceHolder na dwóch kontrolkach zawartości zagnieżdżonej strony wzorcowej, podobnie jak w kroku 2. Dodaj również tekst "Hello, from SimpleNestedAlternate!" w kontrolce zawartości, która odnosi się do `MainContent` ContentPlaceHolder na stronie wzorcowej najwyższego poziomu. Po wprowadzeniu tych zmian Nowa zagnieżdżona jednostronicowa Strona wzorcowa powinna wyglądać podobnie do poniższego:

[!code-aspx[Main](nested-master-pages-vb/samples/sample6.aspx)]

Utwórz stronę zawartości o nazwie `Alternate.aspx` w folderze `NestedMasterPages` i powiąż ją z `SimpleNestedAlternate.master` zagnieżdżoną stroną wzorcową. Dodaj tekst "Hello, z alternatywy!" w kontrolce zawartości, która odnosi się do `MainContent`. Rysunek 7 przedstawia `Alternate.aspx` podczas wyświetlania za pomocą projektanta programu Visual Studio.

[![alternatywnej. aspx jest powiązany z stroną wzorcową SimpleNestedAlternate. Master](nested-master-pages-vb/_static/image20.png)](nested-master-pages-vb/_static/image19.png)

**Ilustracja 07**: `Alternate.aspx` jest powiązany ze stroną wzorcową `SimpleNestedAlternate.master` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](nested-master-pages-vb/_static/image21.png))

Porównaj projektanta na rysunku 7 z projektantem na rysunku 6. Obie strony zawartości mają ten sam układ zdefiniowany na stronie wzorcowej najwyższego poziomu (`Simple.master`), a mianowicie tytuł "zagnieżdżonych stron wzorcowych (prosty)". W obu nadrzędnych stronach głównych obu tych elementów określono odrębną zawartość — tekst "Hello, from SimpleNested!". na rysunku 6 i "Witaj, from SimpleNestedAlternate!" na rysunku 7. Te różnice są proste, ale można zwiększyć ten przykład, aby uwzględnić bardziej zrozumiałe różnice. Na przykład strona `SimpleNested.master` może zawierać menu z opcjami specyficznymi dla jego stron zawartości, natomiast `SimpleNestedAlternate.master` mogą zawierać informacje dotyczące stron zawartości, które są z nią powiązane.

Teraz wyobraź sobie, że firma Microsoft musiała wprowadzić zmiany w układzie przemieszczenia. Załóżmy na przykład, że chcemy dodać listę typowych linków do wszystkich stron zawartości. Aby to osiągnąć, zaktualizujemy stronę wzorcową najwyższego poziomu `Simple.master`. Wszelkie zmiany zostaną natychmiast odzwierciedlone na zagnieżdżonych stronach wzorcowych, a po rozszerzeniu ich stron zawartości.

Aby zademonstrować, w jaki sposób można zmienić układ witryny, należy otworzyć `Simple.master` stronę wzorcową i dodać następujące znaczniki między `topContent` i `mainContent` elementów `<div>`:

[!code-aspx[Main](nested-master-pages-vb/samples/sample7.aspx)]

Spowoduje to dodanie dwóch linków do górnej części każdej strony, która wiąże się z `Simple.master`, `SimpleNested.master`lub `SimpleNestedAlternate.master`; te zmiany dotyczą wszystkich zagnieżdżonych stron wzorcowych i ich stron zawartości natychmiast. Rysunek 8 przedstawia `Alternate.aspx`, gdy jest wyświetlany za pomocą przeglądarki. Zwróć uwagę na dodanie linków w górnej części strony (w porównaniu do rysunku 7).

[![zmienione na stronę wzorcową najwyższego poziomu są natychmiast odzwierciedlane na zagnieżdżonych stronach wzorcowych i ich stronach zawartości](nested-master-pages-vb/_static/image23.png)](nested-master-pages-vb/_static/image22.png)

**Ilustracja 08**: zmiana na stronę wzorcową najwyższego poziomu jest natychmiast odzwierciedlana na zagnieżdżonych stronach wzorcowych i ich stronach zawartości ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](nested-master-pages-vb/_static/image24.png))

## <a name="using-a-nested-master-page-for-the-administration-section"></a>Używanie zagnieżdżonej strony wzorcowej dla sekcji Administracja

W tym momencie oglądamy zalety zagnieżdżonych stron wzorcowych i zaobserwowano, jak tworzyć i używać ich w aplikacji ASP.NET. Przykłady w krokach 1, 2 i 3, jednak polegają na tworzeniu nowej strony wzorcowej najwyższego poziomu, nowych zagnieżdżonych stronach głównych i nowych stronach zawartości. Jak dodać nową zagnieżdżoną stronę wzorcową do witryny sieci Web z istniejącą stroną wzorcową najwyższego poziomu i stronami zawartości?

Integracja zagnieżdżonej strony wzorcowej z istniejącą witryną sieci Web i skojarzenie jej z istniejącymi stronami zawartości wymaga większego nakładu pracy niż od podstaw. Kroki 4, 5, 6 i 7 eksplorują te wyzwania w miarę rozszerzania aplikacji demonstracyjnej, aby dołączyć nową zagnieżdżoną stronę wzorcową o nazwie `AdminNested.master`, która zawiera instrukcje dla administratora i jest używana przez strony ASP.NET w folderze `~/Admin`.

Integracja zagnieżdżonej strony wzorcowej z naszą aplikacją demonstracyjną wprowadza następujące progi:

- Istniejące strony zawartości w folderze `~/Admin` mają pewne oczekiwania ze strony głównej. W przypadku elementów uruchamiających oczekuje się, że niektóre kontrolki ContentPlaceHolder mają być obecne. Ponadto strony `~/Admin/AddProduct.aspx` i `~/Admin/Products.aspx` wywołują metodę `RefreshRecentProductsGrid` publicznego strony wzorcowej, ustawiać jej Właściwość `GridMessageText` lub mieć procedurę obsługi zdarzeń dla zdarzenia `PricesDoubled`. W związku z tym, nasza zagnieżdżona Strona wzorcowa musi udostępniać te same elementy ContentPlaceHolders i publiczną składową.
- W poprzednim samouczku rozszerzono klasę `BasePage`, aby dynamicznie ustawiać właściwość `MasterPageFile` obiektu `Page` na podstawie zmiennej sesji. Jak obsługujemy dynamiczne strony wzorcowe w przypadku używania zagnieżdżonych stron wzorcowych?

Te dwa wyzwania będą się pojawiać w miarę kompilowania zagnieżdżonej strony wzorcowej i używania jej z istniejących stron zawartości. Będziemy badać i surmount te problemy w miarę ich istnienia.

## <a name="step-4-creating-the-nested-master-page"></a>Krok 4. Tworzenie zagnieżdżonej strony wzorcowej

Najpierw należy utworzyć zagnieżdżoną stronę wzorcową, która będzie używana przez strony w sekcji Administracja. Zgodnie z opisem w kroku 2, gdy dodajesz nową zagnieżdżoną stronę wzorcową, musimy określić nadrzędną stronę wzorcową strony wzorcowej. Jednak mamy dwie strony wzorcowe najwyższego poziomu: `Site.master` i `Alternate.master`. Odwołaj, że utworzyliśmy `Alternate.master` w poprzednim samouczku i napisany kod w klasie `BasePage`, która ustawia właściwość `MasterPageFile` obiektu `Page` w czasie wykonywania do `Site.master` lub `Alternate.master` w zależności od wartości zmiennej `MyMasterPage`ej sesji.

Jak skonfigurować naszą zagnieżdżoną stronę wzorcową tak, aby korzystała z odpowiedniej strony wzorcowej najwyższego poziomu? Dostępne są dwie opcje:

- Utwórz dwie zagnieżdżone strony wzorcowe, `AdminNestedSite.master` i `AdminNestedAlternate.master`, a następnie powiąż je z stronami wzorcowymi najwyższego poziomu odpowiednio `Site.master` i `Alternate.master`. W `BasePage`, ustawimy `MasterPageFile` obiektu `Page` na odpowiednią zagnieżdżoną stronę wzorcową.
- Utwórz pojedynczą zagnieżdżoną stronę wzorcową i strony zawartości używają tej konkretnej strony wzorcowej. Następnie w czasie wykonywania musimy ustawić właściwość `MasterPageFile` zagnieżdżonej strony wzorcowej na odpowiednią stronę wzorcową najwyższego poziomu w czasie wykonywania. (Jak już już wiesz, strony wzorcowe mają również właściwość `MasterPageFile`).

Użyjmy drugiej opcji. Utwórz pojedynczy zagnieżdżony plik strony głównej w folderze `~/Admin` o nazwie `AdminNested.master`. Ponieważ zarówno `Site.master`, jak i `Alternate.master` mają ten sam zestaw elementów typu ContentPlaceHolder, nie ma znaczenia, do której strony głównej należy powiązać, chociaż zachęcamy do powiązania go z `Site.master` w celu zapewnienia spójności.

[![dodać zagnieżdżoną stronę wzorcową do folderu ~/Admin.](nested-master-pages-vb/_static/image26.png)](nested-master-pages-vb/_static/image25.png)

**Ilustracja 09**. Dodaj zagnieżdżoną stronę wzorcową do folderu `~/Admin`. ([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](nested-master-pages-vb/_static/image27.png))

Ponieważ zagnieżdżona Strona wzorcowa jest powiązana ze stroną wzorcową z czterema kontrolkami elementów ContentPlaceHolder, program Visual Studio dodaje cztery kontrolki zawartości do nowego, zagnieżdżonego pliku strony głównej. Podobnie jak w przypadku kroków 2 i 3, Dodaj kontrolkę ContentPlaceHolder w każdej kontrolce zawartości, dając ją taką samą nazwę jak formant ContentPlaceHolder strony wzorcowej najwyższego poziomu. Dodaj również następujące znaczniki do kontrolki zawartości, która odnosi się do `MainContent` ContentPlaceHolder:

[!code-html[Main](nested-master-pages-vb/samples/sample8.html)]

Następnie zdefiniuj klasę `instructions` CSS w plikach CSS `Styles.css` i `AlternateStyles.css`. Poniższe reguły CSS powodują, że elementy HTML mają styl z klasą `instructions`, które mają być wyświetlane z jasnym żółtym kolorem tła i czarnym, ciągłym obramowaniem:

[!code-css[Main](nested-master-pages-vb/samples/sample9.css)]

Ponieważ ta funkcja adiustacji została dodana do zagnieżdżonej strony wzorcowej, zostanie wyświetlona tylko na tych stronach, które używają tej zagnieżdżonej strony wzorcowej (tj. stron w sekcji Administracja).

Po wprowadzeniu tych dodatków do zagnieżdżonej strony wzorcowej, jej deklaracyjne znaczniki powinny wyglądać podobnie do następujących:

[!code-aspx[Main](nested-master-pages-vb/samples/sample10.aspx)]

Zwróć uwagę, że każda kontrolka zawartości ma kontrolkę ContentPlaceHolder i że właściwości "`ID`" formantów "ContentPlaceHolder" mają przypisane te same wartości, co odpowiednie kontrolki ContentPlaceHolder na stronie wzorcowej najwyższego poziomu. Ponadto znacznik charakterystyczny dla sekcji Administracja pojawia się w `MainContent` ContentPlaceHolder.

Na rysunku nr 10 przedstawiono `AdminNested.master` zagnieżdżoną stronę wzorcową wyświetlaną za pomocą projektanta programu Visual Studio. Możesz zobaczyć instrukcje w żółtym polu w górnej części kontrolki zawartości `MainContent`.

[![zagnieżdżona Strona wzorcowa rozszerza stronę wzorcową najwyższego poziomu, aby zawierała instrukcje dla administratora.](nested-master-pages-vb/_static/image29.png)](nested-master-pages-vb/_static/image28.png)

**Ilustracja 10**. zagnieżdżona Strona wzorcowa rozszerza stronę wzorcową najwyższego poziomu, aby zawierała instrukcje dla administratora. ([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](nested-master-pages-vb/_static/image30.png))

## <a name="step-5-updating-the-existing-content-pages-to-use-the-new-nested-master-page"></a>Krok 5. aktualizowanie istniejących stron zawartości do korzystania z nowej zagnieżdżonej strony wzorcowej

Po dodaniu nowej strony zawartości do sekcji Administracja musimy powiązać ją ze stroną wzorcową `AdminNested.master`, która została właśnie utworzona. Ale co o istniejących stronach zawartości? Obecnie wszystkie strony zawartości w lokacji pochodzą z klasy `BasePage`, która programowo ustawia stronę wzorcową strony zawartości w czasie wykonywania. Nie jest to zachowanie dla stron zawartości w sekcji Administracja. Zamiast tego chcemy, aby te strony zawartości zawsze używały strony `AdminNested.master`. Na podstawie zagnieżdżonej strony wzorcowej można wybrać odpowiednią stronę zawartości najwyższego poziomu w czasie wykonywania.

Najlepszym sposobem osiągnięcia tego żądanego zachowania jest utworzenie nowej niestandardowej klasy strony podstawowej o nazwie `AdminBasePage`, która rozszerza klasę `BasePage`. `AdminBasePage` może następnie zastępować `SetMasterPageFile` i ustawić `MasterPageFile` obiektu `Page` na wartość zakodowaną "~/Admin/AdminNested.master". W ten sposób każda Strona, która pochodzi od `AdminBasePage` będzie używać `AdminNested.master`, natomiast każda Strona, która pochodzi od `BasePage` będzie miała Właściwość `MasterPageFile` ustawioną dynamicznie na "~/site.master" lub "~/Alternate.master" na podstawie wartości zmiennej sesji `MyMasterPage`.

Zacznij od dodania nowego pliku klasy do folderu `App_Code` o nazwie `AdminBasePage.vb`. `AdminBasePage` zwiększyć `BasePage`, a następnie Zastąp metodę `SetMasterPageFile`. W tej metodzie Przypisz `MasterPageFile` wartość "~/Admin/AdminNested.master". Po wprowadzeniu tych zmian plik klasy powinien wyglądać podobnie do poniższego:

[!code-vb[Main](nested-master-pages-vb/samples/sample11.vb)]

Teraz musimy mieć istniejące strony zawartości w sekcji Administracja, które pochodzą od `AdminBasePage`, a nie `BasePage`. Przejdź do pliku klasy związanej z kodem dla każdej strony zawartości w folderze `~/Admin` i wprowadź tę zmianę. Na przykład w `~/Admin/Default.aspx` zmienić deklarację klasy związanej z kodem z:

[!code-vb[Main](nested-master-pages-vb/samples/sample12.vb)]

Do:

[!code-vb[Main](nested-master-pages-vb/samples/sample13.vb)]

Rysunek 11 przedstawia sposób, w jaki strona wzorcowa najwyższego poziomu (`Site.master` lub `Alternate.master`), zagnieżdżona Strona wzorcowa (`AdminNested.master`) i strony zawartości sekcji administracyjnej są powiązane ze sobą.

[![zagnieżdżona Strona wzorcowa definiuje zawartość specyficzną dla stron w sekcji Administracja.](nested-master-pages-vb/_static/image32.png)](nested-master-pages-vb/_static/image31.png)

**Ilustracja 11**. zagnieżdżona Strona wzorcowa definiuje zawartość specyficzną dla stron w sekcji Administracja ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](nested-master-pages-vb/_static/image33.png))

## <a name="step-6-mirroring-the-master-pages-public-methods-and-properties"></a>Krok 6. dublowanie metod i właściwości publicznej strony wzorcowej

Należy przypomnieć, że strony `~/Admin/AddProduct.aspx` i `~/Admin/Products.aspx` współdziałają programowo ze stroną wzorcową: `~/Admin/AddProduct.aspx` wywołuje metodę `RefreshRecentProductsGrid` publiczną strony wzorcowej i ustawia jej Właściwość `GridMessageText`. `~/Admin/Products.aspx` ma procedurę obsługi zdarzeń dla zdarzenia `PricesDoubled`. W poprzednim samouczku utworzyliśmy klasę `BaseMasterPage` `MustInherit`, która definiuje te publiczne elementy członkowskie.

Na stronach `~/Admin/AddProduct.aspx` i `~/Admin/Products.aspx` przyjęto założenie, że ich Strona główna pochodzi z klasy `BaseMasterPage`. Jednak strona `AdminNested.master`, obecnie rozszerza klasę `System.Web.UI.MasterPage`. W związku z tym podczas odwiedzania `~/Admin/Products.aspx` `InvalidCastException` zostanie zgłoszony komunikat: "nie można rzutować obiektu typu" ASP. Admin\_adminnested\_Master "na typ" BaseMasterPage "."

Aby rozwiązać ten problem, musimy mieć `AdminNested.master` klasy związanej z kodem `BaseMasterPage`. Aktualizowanie deklaracji klasy zagnieżdżonej strony wzorcowej z:

[!code-vb[Main](nested-master-pages-vb/samples/sample14.vb)]

Do:

[!code-vb[Main](nested-master-pages-vb/samples/sample15.vb)]

Nie zostało to jeszcze zrobione. Musimy przesłonić elementy członkowskie oznaczone jako `MustOverride`, mianowicie `RefreshRecentProductsGrid` i `GridMessageText`. Te elementy członkowskie są używane przez strony wzorcowe najwyższego poziomu do aktualizowania interfejsów użytkownika. (W rzeczywistości tylko strona wzorcowa `Site.master` używa tych metod, mimo że obie strony wzorcowe najwyższego poziomu implementują te metody, ponieważ oba rozszerzone `BaseMasterPage`).

Chociaż musimy zaimplementować te elementy członkowskie w `AdminNested.master`, wszystkie te implementacje muszą po prostu wywołać ten sam element członkowski na stronie wzorcowej najwyższego poziomu używanej przez zagnieżdżoną stronę wzorcową. Na przykład, gdy strona zawartości w sekcji Administracja wywołuje metodę `RefreshRecentProductsGrid` zagnieżdżonej strony wzorcowej, cała zagnieżdżona Strona wzorcowa musi mieć wartość, z kolei wywołać metodę `Site.master` lub `Alternate.master``RefreshRecentProductsGrid`.

Aby to osiągnąć, Zacznij od dodania następującej dyrektywy `@MasterType` w górnej części `AdminNested.master`:

[!code-aspx[Main](nested-master-pages-vb/samples/sample16.aspx)]

Odwołaj, że dyrektywa `@MasterType` dodaje silnie wpisaną właściwość do klasy związanej z kodem o nazwie `Master`. Następnie zastąp `RefreshRecentProductsGrid` i `GridMessageText` członków i po prostu Deleguj wywołanie do odpowiedniej metody `Master`:

[!code-vb[Main](nested-master-pages-vb/samples/sample17.vb)]

Mając ten kod na miejscu, powinno być możliwe odwiedzenie i użycie stron zawartości w sekcji Administracja. Na rysunku 12 przedstawiono stronę `~/Admin/Products.aspx` wyświetlaną w przeglądarce. Jak widać, Strona zawiera pole instrukcji administracyjnych zdefiniowane na zagnieżdżonej stronie wzorcowej.

[![strony zawartości w sekcji Administracja zawierają instrukcje w górnej części każdej strony](nested-master-pages-vb/_static/image35.png)](nested-master-pages-vb/_static/image34.png)

**Ilustracja 12**. strony zawartości w sekcji Administracja zawierają instrukcje w górnej części każdej strony ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](nested-master-pages-vb/_static/image36.png))

## <a name="step-7-using-the-appropriate-top-level-master-page-at-runtime"></a>Krok 7. Korzystanie z odpowiedniej strony wzorcowej najwyższego poziomu w środowisku uruchomieniowym

Mimo że wszystkie strony zawartości w sekcji Administracja są w pełni funkcjonalne, wszystkie używają tej samej strony wzorcowej najwyższego poziomu i ignorują stronę wzorcową wybraną przez użytkownika na `ChooseMasterPage.aspx`. To zachowanie jest spowodowane faktem, że zagnieżdżona Strona wzorcowa ma swoją właściwość `MasterPageFile` statycznie ustawiona na `Site.master` w `<%@ Master %>` dyrektywie.

Aby użyć strony wzorcowej najwyższego poziomu wybranej przez użytkownika końcowego, musimy ustawić właściwość `MasterPageFile` `AdminNested.master`na wartość w zmiennej sesji `MyMasterPage`. Ze względu na to, że ustawimy właściwości `MasterPageFile` stron zawartości w `BasePage`, możesz zastanowić się, że ustawimy Właściwość `MasterPageFile` zagnieżdżonej strony wzorcowej w `BaseMasterPage` lub w klasie z kodem związanym `AdminNested.master`. Nie będzie to jednak konieczne, ponieważ musimy ustawić właściwość `MasterPageFile` na końcu etapu wstępnego inicjowania. Najwcześniejszy czas, który można programistycznie nacisnąć w cyklu życia strony z strony wzorcowej, to etap inicjowania (który występuje po etapie inicjowania wstępnego).

W związku z tym musimy ustawić właściwość `MasterPageFile` zagnieżdżonej strony wzorcowej na stronach zawartości. Jedyne strony zawartości, które używają `AdminNested.master` stronie wzorcowej, pochodzą z `AdminBasePage`. W związku z tym możemy umieścić tę logikę w tym miejscu. W kroku 5 overrode metodę `SetMasterPageFile`, ustawiając właściwość `MasterPageFile` obiektu strony na wartość "~/Admin/AdminNested.master". Zaktualizuj `SetMasterPageFile`, aby również ustawić właściwość `MasterPageFile` strony wzorcowej na wynik przechowywany w sesji:

[!code-vb[Main](nested-master-pages-vb/samples/sample18.vb)]

Metoda `GetMasterPageFileFromSession`, która została dodana do klasy `BasePage` w poprzednim samouczku, zwraca odpowiednią ścieżkę pliku strony głównej na podstawie wartości zmiennej sesji.

Po dokonaniu tej zmiany wybór strony głównej użytkownika przenosi się do sekcji Administracja. Rysunek 13 przedstawia tę samą stronę co Rysunek 12, ale po zmianie przez użytkownika opcji wyboru strony wzorcowej na `Alternate.master`.

[![zagnieżdżona Strona administracyjna używa strony wzorcowej najwyższego poziomu wybranej przez użytkownika](nested-master-pages-vb/_static/image38.png)](nested-master-pages-vb/_static/image37.png)

**Ilustracja 13**. zagnieżdżona Strona administracyjna używa strony wzorcowej najwyższego poziomu wybranej przez użytkownika ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](nested-master-pages-vb/_static/image39.png))

## <a name="summary"></a>Podsumowanie

Podobnie jak strony zawartości mogą być powiązane z stroną wzorcową, możliwe jest utworzenie zagnieżdżonych stron wzorcowych, gdy podrzędna Strona wzorcowa jest powiązana z nadrzędną stroną wzorcową. Podrzędna Strona wzorcowa może definiować kontrolki zawartości dla wszystkich elementów ContentPlaceHolder swojego elementu nadrzędnego; może wtedy dodać własne kontrolki ContentPlaceHolder (a także inne znaczniki) do tych kontrolek zawartości. Zagnieżdżone strony wzorcowe są bardzo przydatne w dużych aplikacjach sieci Web, w których wszystkie strony współdzielą wygląd i działanie, ale pewne sekcje lokacji wymagają unikatowych dostosowań.

Szczęśliwe programowanie!

### <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Zagnieżdżone strony wzorcowe ASP.NET](https://msdn.microsoft.com/library/x2b3ktt7.aspx)
- [Wskazówki dotyczące zagnieżdżonych stron wzorcowych i czasu projektowania programu VS 2005](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx)
- [Obsługa zagnieżdżonej strony głównej VS 2008](https://weblogs.asp.net/scottgu/archive/2007/07/09/vs-2008-nested-master-page-support.aspx)

### <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor wielu książek ASP/ASP. NET Books i założyciel of 4GuysFromRolla.com, pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 3,5 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott można uzyskać w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu w [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Ubiegł](specifying-the-master-page-programmatically-vb.md)

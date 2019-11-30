---
uid: web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-vb
title: Stronicowanie i sortowanie danych raportu (VB) | Microsoft Docs
author: rick-anderson
description: Stronicowanie i sortowanie to dwie bardzo popularne funkcje podczas wyświetlania danych w aplikacji online. W tym samouczku zajmiemy się dodawaniem sortowania i...
ms.author: riande
ms.date: 08/15/2006
ms.assetid: b895e37e-0e69-45cc-a7e4-17ddd2e1b38d
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 6785b5cd2d4d3a2c2e7f2c2fea93f5cd5e2fdf24
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74619095"
---
# <a name="paging-and-sorting-report-data-vb"></a>Stronicowanie i sortowanie danych raportu (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_24_VB.exe) lub [Pobierz plik PDF](paging-and-sorting-report-data-vb/_static/datatutorial24vb1.pdf)

> Stronicowanie i sortowanie to dwie bardzo popularne funkcje podczas wyświetlania danych w aplikacji online. W tym samouczku zajmiemy się pierwszym przystąpieniem do dodawania sortowania i stronicowania do raportów, które zostaną następnie skompilowane w przyszłych samouczkach.

## <a name="introduction"></a>Wprowadzenie

Stronicowanie i sortowanie to dwie bardzo popularne funkcje podczas wyświetlania danych w aplikacji online. Na przykład podczas wyszukiwania książek ASP.NET w księgarni online mogą istnieć setki takich książek, ale Raport z listą wyników wyszukiwania dotyczy tylko dziesięciu odpowiedników na stronę. Ponadto wyniki mogą być sortowane według tytułu, ceny, liczby stron, nazwy autora itd. Mimo że ostatnie 23 samouczki sprawdziły, jak tworzyć różne raporty, w tym interfejsy, które umożliwiają dodawanie, edytowanie i usuwanie danych, nie wiemy, jak sortować dane, a jedynie przykłady stronicowania były widoczne w widoku DetailsView i FormView kontrolek.

W tym samouczku dowiesz się, jak dodać sortowanie i stronicowanie do raportów, które można wykonać, zaznaczając kilka pól wyboru. Niestety, ta implementacja uproszczony ma swoje wady, które powoduje, że interfejs sortowania pozostawia żądany bit, a procedury stronicowania nie są przeznaczone do wydajnego stronicowania w dużych zestawach wyników. W przyszłości samouczki zobaczą, jak przezwyciężyć ograniczenia wbudowanych rozwiązań stronicowania i sortowania.

## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>Krok 1. Dodawanie stron sieci Web samouczka stronicowania i sortowania

Przed rozpoczęciem pracy z tym samouczkiem Poświęć chwilę na dodanie ASP.NET stron, których potrzebujemy w tym samouczku, i następnych trzech. Zacznij od utworzenia nowego folderu w projekcie o nazwie `PagingAndSorting`. Następnie Dodaj następujące pięć stron ASP.NET do tego folderu, z których wszystkie są skonfigurowane do korzystania z strony wzorcowej `Site.master`:

- `Default.aspx`
- `SimplePagingSorting.aspx`
- `EfficientPaging.aspx`
- `SortParameter.aspx`
- `CustomSortingUI.aspx`

![Utwórz folder PagingAndSorting i Dodaj samouczek ASP.NET stron](paging-and-sorting-report-data-vb/_static/image1.png)

**Rysunek 1**. Tworzenie PagingAndSorting folderu i Dodawanie samouczka ASP.NET stron

Następnie otwórz stronę `Default.aspx` i przeciągnij kontrolkę użytkownika `SectionLevelTutorialListing.ascx` z folderu `UserControls` na powierzchnię projektu. Ta kontrolka użytkownika, utworzona na [stronach wzorcowych i](../introduction/master-pages-and-site-navigation-vb.md) w samouczku nawigacji po witrynie, wylicza mapę witryny i wyświetla te samouczki w bieżącej sekcji na liście punktowanej.

![Dodaj kontrolkę użytkownika SectionLevelTutorialListing. ascx do default. aspx](paging-and-sorting-report-data-vb/_static/image2.png)

**Rysunek 2**. Dodawanie kontrolki użytkownika SectionLevelTutorialListing. ascx do default. aspx

Aby Lista punktowana wyświetlała wyświetlane przez siebie samouczki i sortowania, należy dodać je do mapy witryny. Otwórz plik `Web.sitemap` i Dodaj następujący znacznik po edycji, wstawieniu i usunięciu znacznika węzła mapy witryny:

[!code-xml[Main](paging-and-sorting-report-data-vb/samples/sample1.xml)]

![Aktualizowanie mapy witryny w celu uwzględnienia nowych stron ASP.NET](paging-and-sorting-report-data-vb/_static/image3.png)

**Rysunek 3**. aktualizowanie mapy witryny w celu uwzględnienia nowych stron ASP.NET

## <a name="step-2-displaying-product-information-in-a-gridview"></a>Krok 2. Wyświetlanie informacji o produkcie w widoku GridView

Przed faktycznym zaimplementowaniem funkcji stronicowania i sortowania, pozwól, aby najpierw utworzyć standardowy, niedrukowalny widok, który zawiera informacje o produkcie. To zadanie jest wykonywane wiele razy przed wykonaniem tej serii samouczków, dlatego należy zapoznać się z tymi krokami. Zacznij od otworzenia strony `SimplePagingSorting.aspx` i przeciągnij kontrolkę GridView z przybornika do projektanta, ustawiając jej Właściwość `ID` na `Products`. Następnie utwórz nowy element ObjectDataSource, który używa metody `GetProducts()` klasy ProductsBLL, aby zwrócić wszystkie informacje o produkcie.

![Pobierz informacje o wszystkich produktach przy użyciu metody getProducts ()](paging-and-sorting-report-data-vb/_static/image4.png)

**Rysunek 4**. Pobieranie informacji o wszystkich produktach przy użyciu metody getProducts ()

Ponieważ ten raport jest raportem tylko do odczytu, nie ma potrzeby mapowania metod "`Insert()`", `Update()`lub `Delete()` do odpowiednich metod `ProductsBLL`; w związku z tym wybierz pozycję (brak) z listy rozwijanej dla kart UPDATE, INSERT i DELETE.

![Wybierz opcję (brak) z listy rozwijanej na kartach aktualizowanie, wstawianie i usuwanie](paging-and-sorting-report-data-vb/_static/image5.png)

**Ilustracja 5**. Wybierz opcję (brak) z listy rozwijanej na kartach aktualizowanie, wstawianie i usuwanie

Następnie pozwól s dostosować pola GridView w taki sposób, aby były wyświetlane tylko nazwy produktów, dostawcy, kategorie, ceny i Stany wycofane. Ponadto możesz swobodnie wprowadzać zmiany w formatowaniu na poziomie pola, takie jak dostosowanie `HeaderText` właściwości lub formatowanie ceny jako waluty. Po wprowadzeniu tych zmian znaczniki deklaratywne GridView powinny wyglądać podobnie do następujących:

[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample2.aspx)]

Rysunek 6 przedstawia postęp z tego względu, gdy jest wyświetlany za pomocą przeglądarki. Zwróć uwagę na to, że strona wyświetla wszystkie produkty na jednym ekranie, pokazując każdy produkt, kategorię, dostawcę, cenę i stan wycofany.

[![wszystkie produkty są wymienione na liście](paging-and-sorting-report-data-vb/_static/image7.png)](paging-and-sorting-report-data-vb/_static/image6.png)

**Ilustracja 6**. wszystkie produkty są wymienione na liście ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](paging-and-sorting-report-data-vb/_static/image8.png))

## <a name="step-3-adding-paging-support"></a>Krok 3. Dodawanie obsługi stronicowania

Lista *wszystkich* produktów na jednym ekranie może prowadzić do przeciążenia informacji dla użytkownika, w którym dane są przeciążać. Aby ułatwić łatwiejsze zarządzanie wynikami, możemy podzielić dane na mniejsze strony danych i umożliwić użytkownikowi przechodzenie między danymi po jednej stronie. Aby to osiągnąć, po prostu zaznacz pole wyboru Włącz stronicowanie z taga inteligentnego "GridView s" (ustawia [właściwość GridView`AllowPaging`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowpaging.aspx) na `true`).

[![zaznacz pole wyboru Włącz stronicowanie, aby dodać obsługę stronicowania](paging-and-sorting-report-data-vb/_static/image10.png)](paging-and-sorting-report-data-vb/_static/image9.png)

**Rysunek 7**. Zaznacz pole wyboru Włącz stronicowanie, aby dodać obsługę stronicowania ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](paging-and-sorting-report-data-vb/_static/image11.png))

Włączenie stronicowania ogranicza liczbę rekordów wyświetlanych na stronie i dodaje *interfejs stronicowania* do widoku GridView. Domyślny interfejs stronicowania, przedstawiony na rysunku 7, to seria numerów stron, dzięki czemu użytkownik może szybko nawigować z jednej strony danych do innej. Ten interfejs stronicowania powinien wyglądać znajomo, ponieważ jest on widoczny podczas dodawania obsługi stronicowania do kontrolek DetailsView i FormView w poprzednich samouczkach.

Zarówno kontrolki DetailsView, jak i FormView pokazują tylko jeden rekord na stronę. W widoku GridView jest jednak sprawdzana [`PageSize` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.pagesize.aspx) , aby określić liczbę rekordów do wyświetlenia na stronie (wartość domyślna to 10).

Ten interfejs stronicowania GridView, DetailsView i FormView można dostosować przy użyciu następujących właściwości:

- `PagerStyle` wskazuje informacje o stylu dla interfejsu stronicowania; może określać ustawienia, takie jak `BackColor`, `ForeColor`, `CssClass`, `HorizontalAlign`i tak dalej.
- `PagerSettings` zawiera bevy właściwości, które mogą dostosować funkcjonalność interfejsu stronicowania; `PageButtonCount` wskazuje maksymalną liczbę liczb stron numerycznych wyświetlanych w interfejsie stronicowania (wartość domyślna to 10); [właściwość`Mode`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pagersettings.mode.aspx) wskazuje, jak działa interfejs stronicowania i można ją ustawić na: 

    - `NextPrevious` Wyświetla następny i Poprzedni przycisk, umożliwiając użytkownikowi przechodzenie do przodu lub do tyłu jednej strony w danym momencie
    - `NextPreviousFirstLast` oprócz przycisków Następny i poprzedni, są również dołączone przyciski pierwszy i ostatni, co umożliwia użytkownikowi szybkie przechodzenie do pierwszej lub ostatniej strony danych
    - `Numeric` przedstawia serię numerów stron, umożliwiając użytkownikowi natychmiastowe przechodzenie do dowolnej strony
    - Oprócz numerów stron, zawiera przyciski pierwszy i ostatni, co umożliwia użytkownikowi szybkie przechodzenie do pierwszej lub ostatniej strony danych; `NumericFirstLast` Pierwszy/ostatni przycisk jest wyświetlany tylko wtedy, gdy nie można dopasować wszystkich liczb stron numerycznych

Ponadto w obszarze GridView, DetailsView i FormView All są oferowane `PageIndex` i `PageCount` właściwości, które wskazują bieżącą przeglądaną stronę i łączną liczbę stron danych. Właściwość `PageIndex` jest indeksowana począwszy od 0, co oznacza, że podczas wyświetlania pierwszej strony `PageIndex` danych będzie równa 0. `PageCount`, z drugiej strony, rozpocznie zliczanie o 1, co oznacza, że `PageIndex` są ograniczone do wartości z zakresu od 0 do `PageCount - 1`.

Poświęć chwilę na zwiększenie domyślnego wyglądu naszego interfejsu stronicowania w widoku GridView. Poinformuj o to, aby interfejs stronicowania był wyrównany do prawej strony przy użyciu jasnego szarego tła. Zamiast ustawiania tych właściwości bezpośrednio za pomocą właściwości `PagerStyle` GridView, Zezwól na tworzenie klasy CSS w `Styles.css` o nazwie `PagerRowStyle`, a następnie przypisz Właściwość `PagerStyle` s `CssClass` za pomocą naszego motywu. Zacznij od otwarcia `Styles.css` i dodania następującej definicji klasy CSS:

[!code-css[Main](paging-and-sorting-report-data-vb/samples/sample3.css)]

Następnie otwórz plik `GridView.skin` w folderze `DataWebControls` w folderze `App_Themes`. Jak opisano w samouczku *strony wzorcowe i nawigacja w witrynie* , można użyć plików karnacji do określenia domyślnych wartości właściwości kontrolki sieci Web. W związku z tym, uzupełnij istniejące ustawienia, aby uwzględnić ustawienie właściwości `CssClass` `PagerStyle` s na `PagerRowStyle`. Ponadto, pozwól, aby skonfigurować interfejs stronicowania, aby pokazywał maksymalnie pięć przycisków stron liczbowych przy użyciu interfejsu stronicowania `NumericFirstLast`.

[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample4.aspx)]

## <a name="the-paging-user-experience"></a>Środowisko użytkownika stronicowania

Na rysunku nr 8 przedstawiono stronę sieci Web, która jest przeglądana za pomocą przeglądarki po sprawdzeniu pola wyboru "Włącz stronicowanie w widoku GridView", a konfiguracje `PagerStyle` i `PagerSettings` zostały wykonane za pomocą pliku `GridView.skin`. Zwróć uwagę, jak wyświetlane są tylko dziesięć rekordów, a interfejs stronicowania wskazuje, że oglądamy pierwszą stronę danych.

[![ze stronicowaniem włączonym, tylko podzbiór rekordów jest wyświetlany w danym momencie](paging-and-sorting-report-data-vb/_static/image13.png)](paging-and-sorting-report-data-vb/_static/image12.png)

**Ilustracja 8**. po włączeniu stronicowania w danym momencie jest wyświetlany tylko podzestaw rekordów ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](paging-and-sorting-report-data-vb/_static/image14.png))

Gdy użytkownik kliknie jeden z numerów stron w interfejsie stronicowania, nastąpi odświeżenie i ponowne załadowanie strony zawierające żądane rekordy strony s. Na rysunku 9 przedstawiono wyniki, które należy wykonać, aby wyświetlić końcową stronę danych. Zwróć uwagę, że Ostatnia strona zawiera tylko jeden rekord; wynika to z faktu, że w sumie występuje 81 rekordów, co spowoduje osiem stron 10 rekordów na stronę oraz jedną stronę z rekordem Lone.

[![kliknięcie numeru strony powoduje ogłoszenie zwrotne i zawiera odpowiedni podzestaw rekordów](paging-and-sorting-report-data-vb/_static/image16.png)](paging-and-sorting-report-data-vb/_static/image15.png)

**Ilustracja 9**. kliknięcie numeru strony powoduje ogłoszenie zwrotne i zawiera odpowiedni podzestaw rekordów ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](paging-and-sorting-report-data-vb/_static/image17.png))

## <a name="paging-s-server-side-workflow"></a>Stronicowanie przepływu pracy po stronie serwera

Gdy użytkownik końcowy kliknie przycisk w interfejsie stronicowania, proces ogłaszania zwrotnego zostanie zakończony i rozpocznie się następujący przepływ pracy po stronie serwera:

1. Wyzwolono zdarzenie `PageIndexChanging` GridView (lub DetailsView lub FormView)
2. Element ObjectDataSource żąda ponownego żądania *wszystkich* danych z logiki biznesowej; wartości właściwości GridView `PageIndex` i `PageSize` są używane do określania, które rekordy zwracane z LOGIKI biznesowej muszą być wyświetlane w widoku GridView
3. Wyzwolono zdarzenie `PageIndexChanged` w widoku GridView

W kroku 2 Element ObjectDataSource żąda ponownego żądania wszystkich danych ze źródła danych. Ten styl stronicowania jest często określany jako *domyślne stronicowanie*, w miarę jak s zachowanie stronicowania używane domyślnie podczas ustawiania właściwości `AllowPaging` na `true`. Z domyślnym stronicowaniem formant sieci Web danych naively pobiera wszystkie rekordy dla każdej strony danych, nawet jeśli tylko podzbiór rekordów jest faktycznie renderowany w kodzie HTML, który został wysłany do przeglądarki. Jeśli dane bazy danych nie są buforowane przez LOGIKI biznesowej lub ObjectDataSource, domyślne stronicowanie nie będzie działało dla dostatecznie dużych zestawów wyników lub dla aplikacji sieci Web z wieloma współbieżnymi użytkownikami.

W następnym samouczku sprawdzimy, jak zaimplementować *niestandardowe stronicowanie*. Za pomocą niestandardowego stronicowania można w tym celu wymusić, aby element ObjectDataSource pobierał tylko dokładny zestaw rekordów wymaganych przez żądaną stronę danych. Jak można wyobrazić, niestandardowe stronicowanie znacznie poprawia wydajność stronicowania w dużych zestawach wyników.

> [!NOTE]
> Gdy domyślne stronicowanie nie jest odpowiednie podczas stronicowania za pomocą wystarczająco dużych zestawów wyników lub dla witryn z wieloma jednoczesnymi użytkownikami, należy pamiętać, że niestandardowe stronicowanie wymaga większej liczby zmian i wysiłku w celu zaimplementowania i nie jest tak proste jak sprawdzanie pola wyboru (domyślnie stronicowanie). W związku z tym domyślne stronicowanie może być idealnym wyborem dla małych i niewielkich witryn sieci Web, a także w przypadku odczytywania ich za pomocą stosunkowo małych zestawów wyników.

Jeśli na przykład firma Microsoft wie, że nigdy nie będziemy mieć więcej niż 100 produktów w naszej bazie danych, minimalny wpływ na wydajność spowodowany przez stronicowanie niestandardowe będzie prawdopodobnie przesunięty o nakład pracy wymagany do jego wdrożenia. Jeśli jednak firma Microsoft może w jednym dniu mieć tysiące lub dziesiątki tysięcy produktów, *nie* implementuje stronicowania niestandardowego, znacznie hamuje skalowalność naszej aplikacji.

## <a name="step-4-customizing-the-paging-experience"></a>Krok 4. Dostosowywanie środowiska stronicowania

Kontrolki sieci Web danych udostępniają wiele właściwości, których można użyć do zwiększenia środowiska stronicowania użytkownika. Właściwość `PageCount`, na przykład wskazuje liczbę stron, które znajdują się w tym miejscu, podczas gdy właściwość `PageIndex` wskazuje bieżącą stronę, która jest odwiedzona i można ją ustawić, aby szybko przenieść użytkownika na określoną stronę. Aby zilustrować, jak używać tych właściwości do ulepszania środowiska stronicowania użytkownika, Dodaj kontrolkę sieci Web etykieta do naszej strony, która informuje użytkownika o tym, jakie strony odwiedzają obecnie, wraz z kontrolką DropDownList, która umożliwia im szybkie przechodzenie do dowolnej strony. .

Najpierw Dodaj kontrolkę sieci Web etykieta do strony, ustaw jej Właściwość `ID` na `PagingInformation`i wyczyść Właściwość `Text`. Następnie Utwórz procedurę obsługi zdarzeń dla zdarzenia GridView `DataBound` i Dodaj następujący kod:

[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample5.vb)]

Ta procedura obsługi zdarzeń przypisuje Właściwość `PagingInformation` etykieta s `Text` do wiadomości informującej o tym, że strona, do której są aktualnie odwiedzane, `Products.PageIndex + 1` od liczby stron, które są `Products.PageCount` (dodajemy 1 do właściwości `Products.PageIndex`, ponieważ `PageIndex` jest indeksowana od 0). Po wybraniu właściwości Przypisz tę etykietę `Text` w obsłudze zdarzeń `DataBound`, a nie do programu obsługi zdarzeń `PageIndexChanged`, ponieważ zdarzenie `DataBound` wyzwalane za każdym razem, gdy dane są powiązane z elementem GridView, a program obsługi zdarzeń `PageIndexChanged` jest uruchamiany tylko wtedy, gdy indeks strony zostanie zmieniony. Gdy GridView jest początkowo powiązane z danymi na pierwszej stronie, odwiedzanie zdarzenia `PageIndexChanging` nie jest wyzwalane (podczas gdy `DataBound` zdarzenia).

Dzięki temu użytkownikowi zostanie wyświetlony komunikat informujący o tym, którą stronę odwiedzasz i ile razem zawiera łączną liczbę stron danych.

[![bieżący numer strony i całkowita liczba wyświetlanych stron](paging-and-sorting-report-data-vb/_static/image19.png)](paging-and-sorting-report-data-vb/_static/image18.png)

**Ilustracja 10**. wyświetlane są bieżące numery stron i łączna liczba stron ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](paging-and-sorting-report-data-vb/_static/image20.png))

Oprócz kontrolki etykieta, pozwól również dodać kontrolkę DropDownList, która wyświetla listę numerów stron w widoku GridView z wybraną aktualnie wyświetlaną stroną. Dobrym pomysłem jest to, że użytkownik może szybko przeskoczyć od bieżącej strony do innej, wybierając nowy indeks strony z DropDownList. Zacznij od dodania DropDownList do projektanta, ustawiając jego właściwość `ID` na `PageList` i sprawdzając opcję Włącz autoogłaszanie przy użyciu tagu inteligentnego.

Następnie wróć do programu obsługi zdarzeń `DataBound` i Dodaj następujący kod:

[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample6.vb)]

Ten kod rozpoczyna się od usunięcia elementów w `PageList` DropDownList. Może to pozornie być niepotrzebne, ponieważ nikt nie będzie oczekiwał liczby stron do zmiany, ale inni użytkownicy mogą używać systemu jednocześnie, dodając lub usuwając rekordy z tabeli `Products`. Takie wstawienia lub usunięcia mogą zmienić liczbę stron danych.

Następnie musimy ponownie utworzyć numery stron i mieć te, które są domyślnie mapowane na aktualnie wybrane `PageIndex` GridView. Wykonujemy to z pętlą od 0 do `PageCount - 1`, dodając nowe `ListItem` w każdej iteracji i ustawiając jej Właściwość `Selected` na wartość true, jeśli bieżący indeks iteracji jest równy właściwości GridView s `PageIndex`.

Na koniec musimy utworzyć procedurę obsługi zdarzeń dla zdarzenia `SelectedIndexChanged` s DropDownList, które będzie wyzwalane za każdym razem, gdy użytkownik wybierze inny element z listy. Aby utworzyć ten program obsługi zdarzeń, po prostu kliknij dwukrotnie DropDownList w projektancie, a następnie Dodaj następujący kod:

[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample7.vb)]

Jak pokazano na rysunku 11, tylko zmiana właściwości `PageIndex` GridView powoduje, że dane zostaną przepowiązane z elementem GridView. W programie obsługi zdarzeń `DataBound` GridView jest zaznaczone odpowiednie `ListItem` DropDownList.

[![użytkownik zostanie automatycznie przełączony do szóstej strony podczas wybierania elementu listy rozwijanej Strona 6](paging-and-sorting-report-data-vb/_static/image22.png)](paging-and-sorting-report-data-vb/_static/image21.png)

**Ilustracja 11**. użytkownik jest automatycznie pobierany do szóstej strony podczas wybierania elementu listy rozwijanej Strona 6 (kliknij,[Aby wyświetlić obraz o pełnym rozmiarze](paging-and-sorting-report-data-vb/_static/image23.png))

## <a name="step-5-adding-bi-directional-sorting-support"></a>Krok 5. Dodawanie obsługi sortowania dwukierunkowego

Dodanie obsługi sortowania dwukierunkowego jest tak proste, jak dodanie obsługi stronicowania po prostu zaznacz opcję Włącz sortowanie z taga inteligentnego w widoku GridView, który ustawia właściwość GridView- [`AllowSorting`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowsorting.aspx) na `true`). Renderuje każdy z nagłówków pól GridView jako LinkButtons, które po kliknięciu, powodują ogłaszanie zwrotne i zwracają dane posortowane według klikniętej kolumny w kolejności rosnącej. Kliknięcie tego samego nagłówka element LinkButton ponownie posortuje dane w kolejności malejącej.

> [!NOTE]
> Jeśli używasz niestandardowej warstwy dostępu do danych, a nie zestawu danych, możesz nie mieć opcji Włącz sortowanie w tagu inteligentnym GridView. Tylko okienka GridView powiązane z źródłami danych, które natywnie obsługują sortowanie, mają dostępne to pole wyboru. Typ zestawu danych zapewnia wbudowaną obsługę sortowania, ponieważ element DataTable ADO.NET udostępnia metodę `Sort`, która po wywołaniu sortuje wiersze datadatatable, używając określonych kryteriów.

Jeśli DAL nie zwraca obiektów, które natywnie obsługują sortowanie, należy skonfigurować element ObjectDataSource do przekazywania informacji o sortowaniu do warstwy logiki biznesowej, która może sortować dane lub mieć dane posortowane przez DAL. Dowiesz się, jak sortować dane za pomocą logiki biznesowej i warstw dostępu do danych w przyszłym samouczku.

LinkButtons sortowania są renderowane jako hiperłącza HTML, których bieżące kolory (niebieskie dla nieodwiedzonego linku i ciemne czerwona dla odwiedzonego linku) kolidują z kolorem tła wiersza nagłówka. W zamian wszystkie linki wierszy nagłówka są wyświetlane w kolorze białym, bez względu na to, czy zostały one odwiedzone. Można to osiągnąć przez dodanie następujących do klasy `Styles.css`:

[!code-css[Main](paging-and-sorting-report-data-vb/samples/sample8.css)]

Ta składnia wskazuje, aby użyć białego tekstu podczas wyświetlania tych hiperłączy w elemencie, który używa klasy nagłówkowe.

Po dodaniu tej CSS podczas odwiedzania strony za pomocą przeglądarki ekran powinien wyglądać podobnie do rysunku 12. W szczególności Rysunek 12 przedstawia wyniki po kliknięciu łącza do nagłówka pola Cena.

[![wyniki zostały posortowane według ceny jednostkowej w kolejności rosnącej](paging-and-sorting-report-data-vb/_static/image25.png)](paging-and-sorting-report-data-vb/_static/image24.png)

**Ilustracja 12**. wyniki zostały posortowane według wartości jednostkowej w kolejności rosnącej ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](paging-and-sorting-report-data-vb/_static/image26.png))

## <a name="examining-the-sorting-workflow"></a>Badanie przepływu pracy sortowania

Wszystkie pola GridView, BoundField, CheckBoxField, TemplateField i tak dalej, mają właściwość `SortExpression`, która wskazuje wyrażenie, które ma być używane do sortowania danych, gdy kliknięto łącze nagłówka sortowania pól s. Widok GridView ma również właściwość `SortExpression`. Po kliknięciu nagłówka sortowania element LinkButton, GridView przypisuje `SortExpression` wartość pola do właściwości `SortExpression`. Następnie dane są pobierane ponownie z elementu ObjectDataSource i sortowane według właściwości `SortExpression` GridView. Poniższa lista zawiera szczegółowe informacje na temat sekwencji kroków, które transpires, gdy użytkownik końcowy sortuje dane w widoku GridView:

1. Wyzwolono [zdarzenie sortowania](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorting(VS.80).aspx) GridView s
2. [Właściwość GridView`SortExpression`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) jest ustawiona na `SortExpression` pola, którego nagłówek sortowania element LinkButton został kliknięty
3. Element ObjectDataSource ponownie pobiera wszystkie dane z LOGIKI biznesowej, a następnie sortuje dane przy użyciu `SortExpression` GridView
4. Właściwość GridView s `PageIndex` jest resetowana do wartości 0, co oznacza, że podczas sortowania użytkownika jest zwracana do pierwszej strony danych (przy założeniu, że obsługa stronicowania została zaimplementowana)
5. Wyzwolono [zdarzenie`Sorted`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorted(VS.80).aspx) w widoku GridView

Podobnie jak w przypadku domyślnego stronicowania, opcja sortowania domyślnie pobiera *wszystkie* rekordy z logiki biznesowej. W przypadku korzystania z sortowania bez stronicowania lub sortowania przy użyciu domyślnego stronicowania nie ma możliwości obejścia tego trafienia wydajności (w skrócie pamięci podręcznej danych bazy danych). Jednak w przyszłości zobaczymy, że będziemy mogli efektywnie sortować dane przy użyciu stronicowania niestandardowego.

W przypadku powiązania elementu ObjectDataSource z GridView przez listę rozwijaną w tagu inteligentnym GridView, każde pole GridView ma automatycznie przypisaną Właściwość `SortExpression` do nazwy pola danych w klasie `ProductsRow`. Na przykład `ProductName` BoundField s `SortExpression` jest ustawiona na `ProductName`, jak pokazano w następujących deklaracyjne znaczniki:

[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample9.aspx)]

Pole można skonfigurować tak, aby nie było możliwe do odsortowania przez wyczyszczenie jego właściwości `SortExpression` (przypisanie go do pustego ciągu). Aby to zilustrować, Wyobraź sobie, że nie chcemy pozwolić naszym klientom na sortowanie naszych produktów według cen. Właściwość `UnitPrice` BoundField s `SortExpression` można usunąć z znaczników deklaratywnych lub za pomocą okna dialogowego pola (dostępnego po kliknięciu łącza Edytuj kolumny w tagu inteligentnym GridView).

![Wyniki zostały posortowane według ceny jednostkowej w kolejności rosnącej](paging-and-sorting-report-data-vb/_static/image27.png)

**Ilustracja 13**. wyniki zostały posortowane według wartości jednostkowej w kolejności rosnącej

Po usunięciu `SortExpression` właściwości dla `UnitPrice` BoundField, nagłówek jest renderowany jako tekst, a nie jako link, uniemożliwiając użytkownikom sortowanie danych według ceny.

[![, usuwając Właściwość 'Sortexpression, użytkownicy nie będą już sortować produktów według ceny](paging-and-sorting-report-data-vb/_static/image29.png)](paging-and-sorting-report-data-vb/_static/image28.png)

**Ilustracja 14**. usunięcie właściwości 'Sortexpression użytkownicy nie mogą już sortować produktów według ceny ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](paging-and-sorting-report-data-vb/_static/image30.png))

## <a name="programmatically-sorting-the-gridview"></a>Programistyczne sortowanie widoku GridView

Zawartość widoku GridView można także sortować programowo przy użyciu [metody`Sort`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sort.aspx)GridView. Po prostu przekaż wartość `SortExpression`, aby posortować według [`SortDirection`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sortdirection.aspx) (`Ascending` lub `Descending`), a dane GridView zostaną posortowane.

Załóżmy, że powód, dla którego wystąpiło sortowanie według `UnitPrice`, został martwisz się, że nasi klienci po prostu kupują tylko produkty o najniższych cenach. Chcemy jednak zachęcić użytkowników do kupowania najbardziej kosztownych produktów, więc lubimy, aby posortować produkty według ceny, ale tylko od najbardziej kosztownej ceny do najmniej.

Aby to zrobić, Dodaj kontrolkę sieci Web przycisku do strony, ustaw jej Właściwość `ID` na `SortPriceDescending`, a jej Właściwość `Text` umożliwia sortowanie według cen. Następnie Utwórz procedurę obsługi zdarzeń dla zdarzenia `Click` przycisku, klikając dwukrotnie formant Button w projektancie. Dodaj następujący kod do tej procedury obsługi zdarzeń:

[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample10.vb)]

Kliknięcie tego przycisku zwraca użytkownika do pierwszej strony z produktami posortowanymi według ceny, od najtańszych do najtańszych (zobacz rysunek 15).

[![kliknięcie przycisku powoduje zamówienie produktów od najtańszych do najmniej](paging-and-sorting-report-data-vb/_static/image32.png)](paging-and-sorting-report-data-vb/_static/image31.png)

**Ilustracja 15**. kliknięcie przycisku powoduje zamówienie produktów od najtańszych do najmniej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](paging-and-sorting-report-data-vb/_static/image33.png))

## <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono sposób implementacji domyślnych funkcji stronicowania i sortowania, z których oba były łatwe do sprawdzenia pola wyboru. Gdy użytkownik sortuje lub strony za pomocą danych, podobne przepływy pracy są niezgięcie:

1. Ogłaszanie zwrotne wynikający z
2. Wyzwalane jest zdarzenie przedpoziomu (`PageIndexChanging` lub `Sorting`) kontrolki Web Data
3. Wszystkie dane są pobierane do ponownego pobrania przez element ObjectDataSource
4. Wyzwalane przez formant sieci Web danych zdarzenie na poziomie post (`PageIndexChanged` lub `Sorted`)

Podczas implementowania podstawowego stronicowania i sortowania jest Breeze, aby zwiększyć efektywność pracy przy użyciu bardziej wydajnego niestandardowego stronicowania lub aby dokładniej ulepszyć interfejs stronicowania lub sortowania. W przyszłości samouczki zapoznają się z tymi tematami.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Poprzednie](creating-a-customized-sorting-user-interface-cs.md)
> [dalej](efficiently-paging-through-large-amounts-of-data-vb.md)

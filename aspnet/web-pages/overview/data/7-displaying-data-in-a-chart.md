---
uid: web-pages/overview/data/7-displaying-data-in-a-chart
title: Wyświetlanie danych na wykresie przy użyciu stron sieci Web ASP.NET (Razor) | Microsoft Docs
author: microsoft
description: W tym rozdziale wyjaśniono, jak wyświetlać dane na wykresie. W poprzednich działach przedstawiono sposób ręcznego wyświetlania danych i w siatce. W tym rozdziale wyjaśniono...
ms.author: riande
ms.date: 05/22/2012
ms.assetid: f889fd46-4dac-4ecb-83d8-60e64c22036e
msc.legacyurl: /web-pages/overview/data/7-displaying-data-in-a-chart
msc.type: authoredcontent
ms.openlocfilehash: 6dad67d4e3d38d57a761c567d937d714a3184ea9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78627492"
---
# <a name="displaying-data-in-a-chart-with-aspnet-web-pages-razor"></a>Wyświetlanie danych na wykresie przy użyciu stron sieci Web ASP.NET (Razor)

przez [firmę Microsoft](https://github.com/microsoft)

> W tym artykule wyjaśniono, jak używać wykresu do wyświetlania danych w witrynie internetowej ASP.NET Web Pages (Razor) za pomocą pomocnika `Chart`.
> 
> Dowiesz **się**:
> 
> - Jak wyświetlić dane na wykresie.
> - Tworzenie stylów wykresów przy użyciu wbudowanych motywów.
> - Jak zaoszczędzić wykresy i jak buforować je w celu zapewnienia lepszej wydajności.
> 
> Są to funkcje programowania ASP.NET wprowadzone w artykule:
> 
> - Pomocnik `Chart`.
> 
> > [!NOTE]
> > Informacje przedstawione w tym artykule dotyczą ASP.NET Web Pages 1,0 i Web Pages 2.

<a id="The_Chart_Helper"></a>
## <a name="the-chart-helper"></a>Pomocnik wykresu

Aby wyświetlić dane w formie graficznej, można użyć pomocnika `Chart`. Pomocnik `Chart` może renderować obraz, który wyświetla dane w różnych typach wykresów. Obsługuje wiele opcji formatowania i etykietowania. Pomocnik `Chart` może renderować więcej niż 30 typów wykresów, w tym wszystkie typy wykresów, które mogą być znane z programu Microsoft Excel lub inne wykresy warstwowe narzędzi &#8212; , wykresy słupkowe, wykresy liniowe i wykresy kołowe oraz bardziej wyspecjalizowane wykresy, takie jak wykresy giełdowe.

| **Wykres warstwowy** ![opis: obraz typu wykresu warstwowego](7-displaying-data-in-a-chart/_static/image1.jpg) | **Wykres słupkowy** ![opis: obraz typu wykresu słupkowego](7-displaying-data-in-a-chart/_static/image2.jpg) |
| --- | --- |
| **Wykres kolumnowy** ![opis: obraz typu wykresu kolumnowego](7-displaying-data-in-a-chart/_static/image3.jpg) | **Wykres liniowy** ![opis: obraz typu wykresu liniowego](7-displaying-data-in-a-chart/_static/image4.jpg) |
| Opis ![**wykresu kołowego** : obraz typu wykresu kołowego](7-displaying-data-in-a-chart/_static/image5.jpg) | Opis ![**wykresu giełdowego** : obraz typu wykresu giełdowego](7-displaying-data-in-a-chart/_static/image6.jpg) |

### <a name="chart-elements"></a>Elementy wykresu

Wykresy przedstawiają dane i dodatkowe elementy, takie jak legendy, osie, serie i tak dalej. Na poniższej ilustracji przedstawiono wiele elementów wykresu, które można dostosować podczas korzystania z pomocnika `Chart`. W tym artykule pokazano, jak ustawić niektóre (nie wszystkie) tych elementów.

![Opis: obraz przedstawiający elementy wykresu](7-displaying-data-in-a-chart/_static/image7.jpg)

<a id="Creating_a_Chart"></a>
## <a name="creating-a-chart-from-data"></a>Tworzenie wykresu na podstawie danych

Dane wyświetlane na wykresie mogą pochodzić z tablicy, od wyników zwróconych z bazy danych lub z danych znajdujących się w pliku XML.

### <a name="using-an-array"></a>Korzystanie z tablicy

Zgodnie z opisem w temacie [wprowadzenie do ASP.NET stron sieci Web przy użyciu składni Razor](https://go.microsoft.com/fwlink/?LinkId=202890), tablica umożliwia przechowywanie kolekcji podobnych elementów w jednej zmiennej. Tablice mogą zawierać dane, które mają zostać uwzględnione na wykresie.

Ta procedura przedstawia sposób tworzenia wykresu na podstawie danych w tablicach przy użyciu domyślnego typu wykresu. Przedstawiono w nim również sposób wyświetlania wykresu na stronie.

1. Utwórz nowy plik o nazwie *ChartArrayBasic. cshtml*.
2. Zastąp istniejącą zawartość następującym: 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample1.cshtml)]

    Kod najpierw tworzy nowy wykres i ustawia jego szerokość i wysokość. Należy określić tytuł wykresu przy użyciu metody `AddTitle`. Aby dodać dane, użyj metody `AddSeries`. W tym przykładzie użyto parametrów `name`, `xValue` i `yValues` metody `AddSeries`. Parametr `name` jest wyświetlany w legendzie wykresu. Parametr `xValue` zawiera tablicę danych wyświetlaną wzdłuż osi poziomej wykresu. Parametr `yValues` zawiera tablicę danych, która jest używana do wykreślania pionowych punktów wykresu.

    Metoda `Write` w rzeczywistości renderuje wykres. W tym przypadku, ponieważ nie określono typu wykresu, pomocnik `Chart` renderuje swój domyślny wykres, który jest wykresem kolumnowym.
3. Uruchom stronę w przeglądarce. Przeglądarka wyświetla wykres. 

    ![](7-displaying-data-in-a-chart/_static/image8.jpg)

### <a name="using-a-database-query-for-chart-data"></a>Używanie zapytania bazy danych na potrzeby danych wykresu

Jeśli informacje, które chcesz utworzyć, są zawarte w bazie danych programu, możesz uruchomić zapytanie bazy danych, a następnie użyć danych z wyników do utworzenia wykresu. Ta procedura pokazuje, jak odczytywać i wyświetlać dane z bazy danych utworzonej w artykule [wprowadzenie do pracy z bazą danych w witrynach sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202893).

1. Jeśli folder nie istnieje, Dodaj folder *danych\_aplikacji* do katalogu głównego witryny sieci Web.
2. W folderze *dane\_aplikacji* Dodaj plik bazy danych o nazwie *SmallBakery. sdf* , który został opisany w artykule [wprowadzenie do pracy z bazą danych w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202893).
3. Utwórz nowy plik o nazwie *ChartDataQuery. cshtml*.
4. Zastąp istniejącą zawartość następującym:   

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample2.cshtml)]

    Kod najpierw otwiera bazę danych SmallBakery i przypisuje ją do zmiennej o nazwie `db`. Ta zmienna reprezentuje obiekt `Database`, którego można użyć do odczytu i zapisu w bazie danych. Następnie kod uruchamia zapytanie SQL, aby uzyskać nazwę i cenę każdego produktu. Kod tworzy nowy wykres i przekazuje do niego zapytanie bazy danych, wywołując metodę `DataBindTable` wykresu. Ta metoda przyjmuje dwa parametry: parametr `dataSource` jest przeznaczony dla danych z zapytania, a parametr `xField` pozwala ustawić kolumnę danych, która będzie używana dla osi x wykresu.

    Alternatywnie przy użyciu metody `DataBindTable` można użyć metody `AddSeries` pomocnika `Chart`. Metoda `AddSeries` umożliwia ustawienie parametrów `xValue` i `yValues`. Na przykład zamiast używać metody `DataBindTable` w następujący sposób:

    [!code-css[Main](7-displaying-data-in-a-chart/samples/sample3.css)]

    Można użyć metody `AddSeries` podobnej do tego:

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample4.html)]

    Obie renderują te same wyniki. Metoda `AddSeries` jest bardziej elastyczna, ponieważ można jawnie określić typ i dane wykresu, ale metoda `DataBindTable` jest łatwiejsza do użycia, jeśli nie potrzebujesz dodatkowej elastyczności.
5. Uruchom stronę w przeglądarce. 

    ![](7-displaying-data-in-a-chart/_static/image9.jpg)

### <a name="using-xml-data"></a>Korzystanie z danych XML

Trzecią opcją dla wykresów jest użycie pliku XML jako danych wykresu. Wymaga to, aby plik XML miał również plik schematu (plik*XSD* ), który OPISUJE strukturę XML. Ta procedura pokazuje, jak odczytywać dane z pliku XML.

1. W folderze *dane\_aplikacji* Utwórz nowy plik XML o nazwie *Data. XML*.
2. Zastąp istniejący kod XML następującym, który stanowi pewne dane XML o pracownikach w fikcyjnej firmie. 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample5.xml)]
3. W folderze *dane\_aplikacji* Utwórz nowy plik XML o nazwie *Data. xsd*. (Należy pamiętać, że rozszerzenie tego czasu to *XSD*.)
4. Zastąp istniejący kod XML następującym: 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample6.xml)]
5. W folderze głównym witryny sieci Web Utwórz nowy plik o nazwie *ChartDataXML. cshtml*.
6. Zastąp istniejącą zawartość następującym: 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample7.cshtml)]

    Najpierw kod tworzy obiekt `DataSet`. Ten obiekt jest używany do zarządzania danymi, które są odczytywane z pliku XML i organizuje je zgodnie z informacjami w pliku schematu. (Zauważ, że górna część kodu zawiera instrukcję `using SystemData`. Jest to wymagane w celu umożliwienia pracy z obiektem `DataSet`. Aby uzyskać więcej informacji, zobacz [&quot;przy użyciu instrukcji&quot; i w pełni kwalifikowanych nazw](#SB_UsingStatements) w dalszej części tego artykułu.)

    Następnie kod tworzy obiekt `DataView` na podstawie zestawu danych. Widok danych zawiera obiekt, który można powiązać &#8212; z wykresem, czyli odczyt i wykres. Wykres wiąże się z danymi przy użyciu metody `AddSeries`, jak pokazano wcześniej podczas tworzenia wykresów danych tablicy, z tą różnicą, że w tym czasie parametry `xValue` i `yValues` są ustawione na obiekt `DataView`.

    Ten przykład pokazuje również, jak określić konkretny typ wykresu. Gdy dane zostaną dodane w metodzie `AddSeries`, parametr `chartType` jest również ustawiany do wyświetlania wykresu kołowego.
7. Uruchom stronę w przeglądarce. 

    ![](7-displaying-data-in-a-chart/_static/image10.jpg)

> [!TIP]
> 
> <a id="SB_UsingStatements"></a>
> ### <a name="using-statements-and-fully-qualified-names"></a>Instrukcje "Using" i w pełni kwalifikowane nazwy
> 
> .NET Framework, które ASP.NET strony sieci Web z składnia Razor opierają się na wielu tysiącach składników (klas). Aby można było zarządzać nimi w celu współpracy ze wszystkimi tymi klasami, są one zorganizowane w *przestrzenie nazw*, które są nieco podobne do bibliotek. Na przykład przestrzeń nazw `System.Web` zawiera klasy, które obsługują komunikację przeglądarki/serwera, `System.Xml` przestrzeń nazw zawiera klasy, które są używane do tworzenia i odczytywania plików XML, a przestrzeń nazw `System.Data` zawiera klasy umożliwiające pracy z danymi.
> 
> Aby uzyskać dostęp do każdej klasy w .NET Framework, kod musi znać nie tylko nazwę klasy, ale również przestrzeń nazw, w której znajduje się Klasa. Na przykład aby można było użyć pomocnika `Chart`, kod musi znaleźć klasę `System.Web.Helpers.Chart`, która łączy przestrzeń nazw (`System.Web.Helpers`) z nazwą klasy (`Chart`). Nazywa się to w *pełni kwalifikowaną* nazwą &#8212; klasy kompletną, niejednoznaczną lokalizację w ramach ogromnej części .NET Framework. W kodzie, będzie wyglądać następująco:
> 
> `var myChart = new System.Web.Helpers.Chart(width: 600, height: 400) // etc.`
> 
> Jednak jest to nieskomplikowana (i podatna na błędy), aby używać tych długich, w pełni kwalifikowanych nazw, za każdym razem, gdy chcesz odwołać się do klasy lub pomocnika. W związku z tym, aby ułatwić korzystanie z nazw klas, można *zaimportować* odpowiednie przestrzenie nazw, które zwykle są kilku spośród wielu przestrzeni nazw w .NET Framework. Jeśli zaimportowano przestrzeń nazw, można użyć tylko nazwy klasy (`Chart`) zamiast w pełni kwalifikowanej nazwy (`System.Web.Helpers.Chart`). Gdy kod jest uruchamiany i napotyka nazwę klasy, może wyszukać tylko te przestrzenie nazw, które zostały zaimportowane, aby znaleźć tę klasę.
> 
> Gdy używasz stron sieci Web ASP.NET z składnia Razor do tworzenia stron sieci Web, zwykle używasz tego samego zestawu klas za każdym razem, łącznie z klasą `WebPage`, różnymi pomocnikami i tak dalej. Aby zapisać zadania importowania odpowiednich przestrzeni nazw za każdym razem, gdy tworzysz witrynę sieci Web, ASP.NET jest skonfigurowany tak, aby automatycznie importował zestaw podstawowych przestrzeni nazw dla każdej witryny sieci Web. To dlatego, że nie musisz już zajmować się przestrzenią nazw lub zaimportować do tej pory. wszystkie klasy, z którymi pracowałeś, znajdują się w przestrzeniach nazw, które zostały już zaimportowane.
> 
> Jednak czasami trzeba będzie współpracować z klasą, która nie znajduje się w przestrzeni nazw, która jest automatycznie importowana. W takim przypadku można użyć w pełni kwalifikowanej nazwy klasy lub można ręcznie zaimportować przestrzeń nazw, która zawiera klasę. Aby zaimportować przestrzeń nazw, należy użyć instrukcji `using` (`import` w Visual Basic), jak pokazano w przykładzie wcześniej w artykule.
> 
> Na przykład Klasa `DataSet` znajduje się w przestrzeni nazw `System.Data`. Przestrzeń nazw `System.Data` nie jest automatycznie dostępna do ASP.NET stron Razor. W związku z tym, aby współpracować z klasą `DataSet` przy użyciu jej w pełni kwalifikowanej nazwy, można użyć kodu w następujący sposób:
> 
> `var dataSet = new System.Data.DataSet();`
> 
> Jeśli musisz wielokrotnie używać klasy `DataSet`, możesz zaimportować przestrzeń nazw w taki sposób, a następnie użyć samej nazwy klasy w kodzie:
> 
> [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample8.cshtml)]
> 
> Możesz dodać instrukcje `using` dla wszystkich innych przestrzeni nazw .NET Framework, do których chcesz się odwołać. Jednak nie trzeba tego robić często, ponieważ większość klas, z których będziesz korzystać, znajduje się w przestrzeniach nazw, które są importowane automatycznie przez ASP.NET do użycia na stronach *. cshtml* i *. vbhtml* .

<a id="Displaying_Charts"></a>
## <a name="displaying-charts-inside-a-web-page"></a>Wyświetlanie wykresów wewnątrz strony sieci Web

W przykładach, które były już widoczne, tworzysz wykres, a następnie wykres jest renderowany bezpośrednio w przeglądarce jako ilustracja. W wielu przypadkach, mimo że chcesz wyświetlić wykres jako część strony, a nie tylko w przeglądarce. Aby to zrobić, wymaga dwuetapowego procesu. Pierwszym krokiem jest utworzenie strony, która generuje wykres, jak już widzisz.

Drugim krokiem jest wyświetlenie uzyskanego obrazu na innej stronie. Aby wyświetlić obraz, należy użyć elementu HTML `<img>` w taki sam sposób jak w przypadku wyświetlania dowolnego obrazu. Jednak zamiast odwoływania się do pliku *jpg* lub *png* , element `<img>` odwołuje się do pliku *. cshtml* , który zawiera `Chart` pomocnika, który tworzy wykres. Po uruchomieniu strony wyświetlania element `<img>` pobiera dane wyjściowe pomocnika `Chart` i renderuje wykres.

![](7-displaying-data-in-a-chart/_static/image11.jpg)

1. Utwórz plik o nazwie *ShowChart. cshtml*.
2. Zastąp istniejącą zawartość następującym: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample9.html)]

    Kod używa elementu `<img>`, aby wyświetlić wykres utworzony wcześniej w pliku *ChartArrayBasic. cshtml* .
3. Uruchom stronę sieci Web w przeglądarce. Plik *ShowChart. cshtml* wyświetla obraz wykresu na podstawie kodu zawartego w pliku *ChartArrayBasic. cshtml* .

<a id="Styling_a_Chart"></a>
## <a name="styling-a-chart"></a>Określanie stylu wykresu

Pomocnik `Chart` obsługuje dużą liczbę opcji, które umożliwiają dostosowanie wyglądu wykresu. Można ustawić kolory, czcionki, obramowania i tak dalej. Prostą metodą dostosowywania wyglądu wykresu jest użycie *motywu*. Motywy to kolekcje informacji określających sposób renderowania wykresu przy użyciu czcionek, kolorów, etykiet, palet, obramowania i efektów. (Należy zauważyć, że styl wykresu nie wskazuje na typ wykresu).

Poniższa tabela zawiera listę wbudowanych motywów.

| Tematów | Opis |
| --- | --- |
| `Vanilla` | Wyświetla czerwone kolumny na białym tle. |
| `Blue` | Wyświetla niebieskie kolumny na niebieskim tle gradientowym. |
| `Green` | Wyświetla niebieskie kolumny na zielonym tle gradientowym. |
| `Yellow` | Wyświetla pomarańczowe kolumny na żółtym tle gradientu. |
| `Vanilla3D` | Wyświetla czerwone kolumny trójwymiarowe na białym tle. |

Możesz określić motyw, który ma być używany podczas tworzenia nowego wykresu.

1. Utwórz nowy plik o nazwie *ChartStyleGreen. cshtml*.
2. Zastąp istniejącą zawartość na stronie następującymi:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample10.cshtml)]

    Ten kod jest taki sam jak w poprzednim przykładzie, który używa bazy danych dla danych, ale dodaje parametr `theme` podczas tworzenia obiektu `Chart`. Poniżej przedstawiono zmieniony kod:

    [!code-csharp[Main](7-displaying-data-in-a-chart/samples/sample11.cs)]
3. Uruchom stronę w przeglądarce. Zobaczysz te same dane jak wcześniej, ale wykres wygląda bardziej dopracowany: 

    ![](7-displaying-data-in-a-chart/_static/image12.jpg)

<a id="Saving_a_Chart"></a>
## <a name="saving-a-chart"></a>Zapisywanie wykresu

W przypadku korzystania z pomocnika `Chart`, jak dotąd zostało to zrobione w tym artykule, pomocnik odtworzy wykres od początku przy każdym wywołaniu. W razie potrzeby kod wykresu również wykonuje ponowne zapytania do bazy danych lub odczytuje plik XML w celu uzyskania danych. W niektórych przypadkach może to być złożona operacja, na przykład jeśli baza danych, która jest używana do wykonywania zapytań, jest duża, lub jeśli plik XML zawiera wiele danych. Nawet jeśli wykres nie obejmuje dużej ilości danych, proces dynamicznego tworzenia obrazu pobiera zasoby serwera, a wiele osób żąda strony lub stron, na których jest wyświetlany wykres, może mieć wpływ na wydajność witryny sieci Web.

Aby pomóc w zmniejszeniu potencjalnego wpływu na wydajność tworzenia wykresu, można utworzyć wykres, gdy będzie on potrzebny po raz pierwszy, a następnie zapisać go. Gdy wykres będzie ponownie wymagany, a nie jego wygenerowanie, można po prostu pobrać zapisaną wersję i renderować ją.

Wykres można zapisać w następujący sposób:

- Buforowanie wykresu w pamięci komputera (na serwerze).
- Zapisz wykres jako plik obrazu.
- Zapisz wykres jako plik XML. Ta opcja umożliwia zmodyfikowanie wykresu przed jego zapisaniem.

### <a name="caching-a-chart"></a>Buforowanie wykresu

Po utworzeniu wykresu można go buforować. Buforowanie wykresu oznacza, że nie trzeba go ponownie tworzyć, jeśli trzeba będzie go ponownie wyświetlić. Gdy zapisujesz wykres w pamięci podręcznej, nadajesz mu klucz, który musi być unikatowy dla tego wykresu.

Wykresy zapisane w pamięci podręcznej mogą zostać usunięte, jeśli na serwerze jest za mało pamięci. Ponadto pamięć podręczna jest czyszczona, jeśli aplikacja zostanie ponownie uruchomiona z dowolnego powodu. W związku z tym standardowym sposobem pracy z buforowanym wykresem jest zawsze sprawdzenie, czy jest on dostępny w pamięci podręcznej, a jeśli nie, wówczas można go utworzyć lub utworzyć ponownie.

1. W katalogu głównym witryny sieci Web Utwórz plik o nazwie *ShowCachedChart. cshtml*.
2. Zastąp istniejącą zawartość następującym: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample12.html)]

    Tag `<img>` zawiera atrybut `src`, który wskazuje na plik *ChartSaveToCache. cshtml* i przekazuje klucz do strony jako ciąg zapytania. Klucz zawiera wartość &quot;myChartKey&quot;. Plik *ChartSaveToCache. cshtml* zawiera pomocnika `Chart`, który tworzy wykres. Ta strona zostanie utworzona w chwilę.

    Na końcu strony znajduje się link do strony o nazwie *ClearCache. cshtml*. To jest strona, którą utworzysz również wkrótce. Dla tego przykładu potrzebujesz tylko *ClearCache. cshtml* , aby przetworzyć buforowanie. nie jest to link ani Strona, która zwykle jest uwzględniana podczas pracy z buforowanymi wykresami.
3. W katalogu głównym witryny sieci Web Utwórz nowy plik o nazwie *ChartSaveToCache. cshtml*.
4. Zastąp istniejącą zawartość następującym:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample13.cshtml)]

    Kod najpierw sprawdza, czy wszystkie elementy zostały przesłane jako wartość klucza w ciągu zapytania. Jeśli tak, kod próbuje odczytać wykres z pamięci podręcznej, wywołując metodę `GetFromCache` i przekazując klucz. Jeśli wyłączy się, że w pamięci podręcznej nie ma nic w tym kluczu (co się stanie po pierwszym zażądaniu wykresu), kod tworzy wykres w zwykły sposób. Po zakończeniu wykresu kod zapisuje go w pamięci podręcznej, wywołując `SaveToCache`. Ta metoda wymaga klucza (aby można było żądać wykresu później) oraz czas, w którym wykres powinien być zapisany w pamięci podręcznej. (Dokładny czas przetworzenia pamięci podręcznej wykresu zależy od tego, jak często sądzisz, że dane, które reprezentuje, mogą ulec zmianie). Metoda `SaveToCache` wymaga również, aby parametr &#8212; `slidingExpiration`, jeśli jest ustawiona na wartość true, licznik limitu czasu jest resetowany za każdym razem, gdy uzyskuje się dostęp do wykresu. W takim przypadku w efekcie oznacza to, że wpis pamięci podręcznej wykresu wygaśnie 2 minuty po ostatnim uzyskaniu dostępu do wykresu. (Alternatywą dla okresu ważności jest bezwzględne wygaśnięcie, co oznacza, że wpis pamięci podręcznej wygaśnie dokładnie 2 minuty po umieszczeniu w pamięci podręcznej, niezależnie od tego, jak często uzyskano dostęp.)

    Na koniec kod używa metody `WriteFromCache` do pobierania i renderowania wykresu z pamięci podręcznej. Należy zauważyć, że ta metoda jest poza blokiem `if`, który sprawdza pamięć podręczną, ponieważ pobiera wykres z pamięci podręcznej, czy wykres był w tej tabeli, czy musiał zostać wygenerowany i zapisany w pamięci podręcznej.

    Zwróć uwagę, że w przykładzie metoda `AddTitle` obejmuje sygnaturę czasową. (Dodaje bieżącą datę i godzinę &#8212; `DateTime.Now` &#8212; do tytułu).
5. Utwórz nową stronę o nazwie *ClearCache. cshtml* i Zastąp jej zawartość następującym:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample14.cshtml)]

    Ta strona używa pomocnika `WebCache` do usuwania wykresu, który jest buforowany w *ChartSaveToCache. cshtml*. Jak wspomniano wcześniej, zazwyczaj nie trzeba mieć takiej strony. Tworzysz ją tutaj tylko, aby ułatwić testowanie pamięci podręcznej.
6. Uruchom stronę sieci Web *ShowCachedChart. cshtml* w przeglądarce. Na stronie zostanie wyświetlony obraz wykresu na podstawie kodu zawartego w pliku *ChartSaveToCache. cshtml* . Zanotuj, jakie sygnatury czasowe widnieją w tytule wykresu. 

    ![Opis: obraz wykresu podstawowego z sygnaturą czasową w tytule wykresu](7-displaying-data-in-a-chart/_static/image13.jpg)
7. Zamknij okno przeglądarki.
8. Ponownie uruchom *ShowCachedChart. cshtml* . Należy zauważyć, że sygnatura czasowa jest taka sama jak przed, co oznacza, że wykres nie został wygenerowany ponownie, ale zamiast tego jest odczytywany z pamięci podręcznej.
9. W *ShowCachedChart. cshtml*kliknij link **Wyczyść pamięć podręczną** . Spowoduje to przejście do *ClearCache. cshtml*, które informuje, że pamięć podręczna została wyczyszczona.
10. Kliknij link **Wróć do ShowCachedChart. cshtml** lub ponownie uruchom *ShowCachedChart. cshtml* w programie WebMatrix. Należy zauważyć, że ta godzina została zmieniona, ponieważ pamięć podręczna została wyczyszczona. W związku z tym kod musiał ponownie wygenerować wykres i umieścić go z powrotem w pamięci podręcznej.

### <a name="saving-a-chart-as-an-image-file"></a>Zapisywanie wykresu jako pliku obrazu

Wykres można również zapisać jako plik obrazu (na przykład jako plik *jpg* ) na serwerze. Następnie możesz użyć pliku obrazu w taki sam sposób, jak w przypadku dowolnego obrazu. Zalety polega na tym, że plik jest przechowywany zamiast zapisywać w tymczasowej pamięci podręcznej. Nowy obraz wykresu można zapisać w różnych godzinach (na przykład co godzinę), a następnie zachować stały rekord zmian, które wystąpiły w czasie. Pamiętaj, że musisz upewnić się, że aplikacja sieci Web ma uprawnienia do zapisywania pliku w folderze na serwerze, na którym chcesz umieścić plik obrazu.

1. W katalogu głównym witryny sieci Web Utwórz folder o nazwie *\_ChartFiles* , jeśli jeszcze nie istnieje.
2. W katalogu głównym witryny sieci Web Utwórz nowy plik o nazwie *ChartSave. cshtml*.
3. Zastąp istniejącą zawartość następującym:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample15.cshtml)]

    Kod najpierw sprawdza, czy plik *. jpg* istnieje, wywołując metodę `File.Exists`. Jeśli plik nie istnieje, kod tworzy nowy `Chart` z tablicy. Tym razem kod wywołuje metodę `Save` i przekazuje parametr `path`, aby określić ścieżkę pliku i nazwę pliku, w którym ma zostać zapisany wykres. W treści strony element `<img>` używa ścieżki, aby wskazać plik *. jpg* do wyświetlenia.
4. Uruchom plik *ChartSave. cshtml* .
5. Wróć do WebMatrix. Zwróć uwagę, że plik obrazu o nazwie *chart01. jpg* został zapisany w folderze *\_ChartFiles* .

### <a name="saving-a-chart-as-an-xml-file"></a>Zapisywanie wykresu jako pliku XML

Na koniec można zapisać wykres jako plik XML na serwerze. Zaletą użycia tej metody w celu przełączenia wykresu lub zapisania wykresu w pliku jest możliwość modyfikacji kodu XML przed wyświetleniem wykresu, jeśli chcesz. Aplikacja musi mieć uprawnienia do odczytu/zapisu do folderu na serwerze, na którym ma zostać umieszczony plik obrazu.

1. W katalogu głównym witryny sieci Web Utwórz nowy plik o nazwie *ChartSaveXml. cshtml*.
2. Zastąp istniejącą zawartość następującym:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample16.cshtml)]

    Ten kod jest podobny do kodu, który został wcześniej wyświetlony do przechowywania wykresu w pamięci podręcznej, z tą różnicą, że używa pliku XML. Kod najpierw sprawdza, czy plik XML istnieje, wywołując metodę `File.Exists`. Jeśli plik istnieje, kod tworzy nowy obiekt `Chart` i przekazuje nazwę pliku jako parametr `themePath`. Spowoduje to utworzenie wykresu na podstawie dowolnego elementu w pliku XML. Jeśli plik XML jeszcze nie istnieje, kod tworzy wykres, taki jak normalny, a następnie wywołuje `SaveXml`, aby go zapisać. Wykres jest renderowany przy użyciu metody `Write`, tak jak wcześniej.

    Podobnie jak w przypadku strony, która przedstawia buforowanie, ten kod zawiera sygnaturę czasową w tytule wykresu.
3. Utwórz nową stronę o nazwie *ChartDisplayXMLChart. cshtml* i Dodaj do niej następujący znacznik: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample17.html)]
4. Uruchom stronę *ChartDisplayXMLChart. cshtml* . Zostanie wyświetlony wykres. Zanotuj sygnaturę czasową w tytule wykresu.
5. Zamknij okno przeglądarki.
6. W programie WebMatrix kliknij prawym przyciskiem myszy folder *\_ChartFiles* , kliknij przycisk **Odśwież**, a następnie otwórz folder. Plik *xmlchart. XML* w tym folderze został utworzony przez pomocnika `Chart`. 

    ![Opis: folder _ChartFiles przedstawiający plik xmlchart. XML utworzony przez pomocnika wykresu.](7-displaying-data-in-a-chart/_static/image14.jpg)
7. Ponownie uruchom stronę *ChartDisplayXMLChart. cshtml* . Na wykresie jest wyświetlana taka sama sygnatura czasowa pierwszego uruchomienia strony. Dzieje się tak, ponieważ wykres jest generowany na podstawie zapisanego wcześniej kodu XML.
8. W programie WebMatrix otwórz folder *\_ChartFiles* i Usuń plik *xmlchart. XML* .
9. Uruchom ponownie stronę *ChartDisplayXMLChart. cshtml* . Tym razem sygnatura czasowa jest aktualizowana, ponieważ pomocnik `Chart` musiał odtworzyć plik XML. Jeśli chcesz, sprawdź folder *\_ChartFiles* i zwróć uwagę na to, że plik XML jest ponownie.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe materiały

- [Wprowadzenie do pracy z bazą danych w witrynach stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202893)
- [Korzystanie z buforowania w witrynach stron sieci Web ASP.NET w celu zwiększenia wydajności](https://go.microsoft.com/fwlink/?LinkId=202903)
- [Klasa wykresu](https://msdn.microsoft.com/library/system.web.helpers.chart(v=vs.99)) (Dokumentacja interfejsu API stron sieci Web ASP.NET w witrynie MSDN)

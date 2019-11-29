---
uid: web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-cs
title: Buforowanie danych za pomocą elementu ObjectDataSourceC#() | Microsoft Docs
author: rick-anderson
description: Buforowanie może oznaczać różnicę między powolną i szybką aplikacją sieci Web. Ten samouczek to pierwsze z czterech, które szczegółowo zaobserwują buforowanie w ASP.NET...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: bd87413c-8160-4520-a8a2-43b555c4183a
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: c9883314d6153b9816d9bad2a281ab3c0a816448
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74612623"
---
# <a name="caching-data-with-the-objectdatasource-c"></a>Buforowanie danych za pomocą kontrolki ObjectDataSource (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_58_CS.exe) lub [Pobierz plik PDF](caching-data-with-the-objectdatasource-cs/_static/datatutorial58cs1.pdf)

> Buforowanie może oznaczać różnicę między powolną i szybką aplikacją sieci Web. Ten samouczek to pierwsze z czterech, które szczegółowo zaobserwują buforowanie w ASP.NET. Zapoznaj się z kluczowymi założeniami buforowania i sposobem zastosowania buforowania do warstwy prezentacji za pomocą kontrolki ObjectDataSource.

## <a name="introduction"></a>Wprowadzenie

W nauce komputerowej *buforowanie* jest procesem pozyskiwania danych lub informacji, które są kosztowne do uzyskania i przechowywania kopii w lokalizacji, do której jest szybszy dostęp. W przypadku aplikacji opartych na danych duże i złożone zapytania często zużywają większość czasu wykonywania aplikacji. Taka wydajność aplikacji s, a następnie, często można zwiększyć, przechowując wyniki kosztownych zapytań bazy danych w pamięci aplikacji.

ASP.NET 2,0 oferuje różne opcje buforowania. *W pamięci podręcznej*wyrenderowana cała strona sieci Web lub kontrolka użytkownika. Formanty ObjectDataSource i kontrolki SqlDataSource zapewniają również możliwości buforowania, co pozwala na przechowywanie danych w pamięci podręcznej na poziomie kontroli. I ASP.NET s *pamięć podręczna danych* oferuje bogaty interfejs API pamięci podręcznej, który umożliwia deweloperom stron programistyczne buforowanie obiektów. W tym samouczku i następnych trzech będziemy analizować przy użyciu funkcji buforowania elementów ObjectDataSource oraz pamięci podręcznej danych. Dowiesz się również, jak buforować dane w całej aplikacji przy uruchamianiu i jak przechowywać dane w pamięci podręcznej w sposób świeży przy użyciu zależności w przypadku bazy danych SQL. Te samouczki nie eksplorują wyjściowej pamięci podręcznej. Aby zapoznać się ze szczegółowymi informacjami na temat buforowania danych wyjściowych, zobacz [buforowanie danych wyjściowych w ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx).

Buforowanie może być stosowane w dowolnym miejscu architektury, od warstwy dostępu do danych za pośrednictwem warstwy prezentacji. W tym samouczku Przyjrzyjmy się zastosowaniu buforowania do warstwy prezentacji za pomocą kontrolki ObjectDataSource. W następnym samouczku będziemy analizować dane z pamięci podręcznej w warstwie logiki biznesowej.

## <a name="key-caching-concepts"></a>Pojęcia dotyczące buforowania kluczy

Buforowanie może znacznie poprawić ogólną wydajność i skalowalność aplikacji dzięki wykorzystaniu danych, które są kosztowne do generowania i przechowywania kopii w lokalizacji, do której można uzyskać bardziej wydajny dostęp. Ponieważ pamięć podręczna zawiera tylko kopię rzeczywistych danych, dane bazowe mogą stać się nieaktualne lub *przestarzałe*, jeśli dane podstawowe ulegną zmianie. Aby to zwalczać, deweloper strony może wskazać kryteria, za pomocą których element pamięci podręcznej zostanie *wykluczony* z pamięci podręcznej przy użyciu:

- **Kryteria oparte na czasie** element można dodać do pamięci podręcznej przez czas bezwzględny lub ruchomy. Na przykład deweloper strony może wskazywać czas trwania, powiedzmy, 60 sekund. Po absolutnym czasie trwania buforowany element jest wykluczony 60 sekund po dodaniu go do pamięci podręcznej, niezależnie od tego, jak często uzyskano dostęp. Po przejściu w tryb buforowany element jest wykluczany 60 sekund od ostatniego dostępu.
- Zależne od **zależności kryteria** , które można skojarzyć z elementem po dodaniu do pamięci podręcznej. Gdy zmienia się zależność elementu, jest on wykluczony z pamięci podręcznej. Zależność może być plikiem, innym elementem pamięci podręcznej lub kombinacją dwóch. ASP.NET 2,0 umożliwia również zależności pamięci podręcznej SQL, które umożliwiają deweloperom dodanie elementu do pamięci podręcznej i wykluczenie go po zmianie danych źródłowych bazy danych. W nadchodzącym samouczku dotyczącym obsługi pamięci podręcznej SQL zostaną przeszukane zależności dotyczące usługi SQL [cache](using-sql-cache-dependencies-cs.md) .

Niezależnie od określonych kryteriów wykluczenia, element w pamięci podręcznej może być *oczyszczany* przed spełnieniem kryteriów opartych na czasie lub w zależności od czasu. Jeśli pamięć podręczna osiągnęła pojemność, istniejące elementy muszą zostać usunięte, aby można było dodać nowe. W związku z tym podczas programistycznego pracy z pamięcią podręczną, ważne jest, aby zawsze założyć, że dane buforowane mogą nie być obecne. Zobaczymy wzorzec, który będzie używany podczas programistycznego uzyskiwania dostępu do danych z pamięci podręcznej w następnym samouczku, *buforowanie danych w architekturze*.

Buforowanie zapewnia ekonomiczny sposób na ściskanie większej wydajności aplikacji. Jako [Steven Smith](http://aspadvice.com/blogs/ssmith/) przegubs w swoim artykule [ASP.NET buforowanie: techniki i najlepsze rozwiązania](https://msdn.microsoft.com/library/aa478965.aspx):

Buforowanie może być dobrym sposobem na uzyskanie odpowiedniej wydajności bez konieczności długiego czasu i analizy. Pamięć jest w stanie taniej, więc jeśli możesz uzyskać wymaganą wydajność, buforując dane wyjściowe przez 30 sekund zamiast spędzać dzień lub tydzień próbujący zoptymalizować kod lub bazę danych, wykonaj rozwiązanie buforowania (przy założeniu, że 30-sekundowe stare dane jest prawidłowe) i przejdź do. Na koniec niewłaściwy projekt prawdopodobnie zostanie wystawiony do Ciebie, dlatego należy spróbować prawidłowo zaprojektować aplikacje. Ale jeśli musisz już dzisiaj uzyskać wystarczającą wydajność, buforowanie może być doskonałym [podejściem], kupowanie czasu do refaktoryzacji aplikacji w późniejszym czasie, gdy będzie to możliwe.

Chociaż buforowanie może zapewnić znaczne udoskonalenia wydajności, nie dotyczy to wszystkich sytuacji, takich jak aplikacje korzystające z czasu rzeczywistego, często aktualizowane dane lub, gdzie nawet wkrótce nie występują stare dane. Jednak w przypadku większości aplikacji buforowanie powinno być używane. Aby uzyskać więcej informacji na temat buforowania w ASP.NET 2,0, zapoznaj się z sekcją [buforowanie dla wydajności](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/default.aspx) w [samouczkach szybki start](https://quickstarts.asp.net/QuickStartv20/aspnet/)dotyczącej usługi ASP.NET 2,0.

## <a name="step-1-creating-the-caching-web-pages"></a>Krok 1. Tworzenie stron sieci Web pamięci podręcznej

Przed rozpoczęciem eksplorowania funkcji buforowania elementów ObjectDataSource, poświęć chwilę na utworzenie stron ASP.NET w naszym projekcie witryny sieci Web, które będziemy potrzebować do tego samouczka i trzech następnych. Zacznij od dodania nowego folderu o nazwie `Caching`. Następnie Dodaj następujące strony ASP.NET do tego folderu, aby upewnić się, że każda strona jest skojarzona z `Site.master` stroną wzorcową:

- `Default.aspx`
- `ObjectDataSource.aspx`
- `FromTheArchitecture.aspx`
- `AtApplicationStartup.aspx`
- `SqlCacheDependencies.aspx`

![Dodaj strony ASP.NET dla samouczków dotyczących buforowania](caching-data-with-the-objectdatasource-cs/_static/image1.png)

**Rysunek 1**. dodawanie stron ASP.NET dla samouczków dotyczących buforowania

Podobnie jak w przypadku innych folderów, `Default.aspx` w folderze `Caching` wyświetli samouczki w sekcji. Odwołaj się do tego, że formant użytkownika `SectionLevelTutorialListing.ascx` udostępnia tę funkcję. W związku z tym należy dodać tę kontrolkę użytkownika do `Default.aspx`, przeciągając ją z Eksplorator rozwiązań na stronę widok Projekt strony.

[![rysunek 2: Dodawanie kontrolki użytkownika SectionLevelTutorialListing. ascx do default. aspx](caching-data-with-the-objectdatasource-cs/_static/image3.png)](caching-data-with-the-objectdatasource-cs/_static/image2.png)

**Rysunek 2**. rysunek 2: Dodawanie kontrolki użytkownika `SectionLevelTutorialListing.ascx` do `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](caching-data-with-the-objectdatasource-cs/_static/image4.png))

Na koniec Dodaj te strony jako wpisy do pliku `Web.sitemap`. W tym celu Dodaj następujące znaczniki po pracy z danymi binarnymi `<siteMapNode>`:

[!code-xml[Main](caching-data-with-the-objectdatasource-cs/samples/sample1.xml)]

Po aktualizacji `Web.sitemap`zapoznaj się z witryną internetową samouczków za pomocą przeglądarki. Menu po lewej stronie zawiera teraz elementy dla samouczków buforowania.

![Mapa witryny zawiera teraz wpisy dotyczące samouczków buforowania](caching-data-with-the-objectdatasource-cs/_static/image5.png)

**Rysunek 3**. Mapa witryny zawiera teraz wpisy dotyczące samouczków buforowania

## <a name="step-2-displaying-a-list-of-products-in-a-web-page"></a>Krok 2. Wyświetlanie listy produktów na stronie sieci Web

W tym samouczku przedstawiono sposób użycia wbudowanych funkcji pamięci podręcznej formantów ObjectDataSource. Zanim będziemy mogli zapoznać się z tymi funkcjami, najpierw potrzebujemy strony, z której korzystamy. Pozwól usłudze s utworzyć stronę sieci Web, która używa widoku GridView do wyświetlania informacji o produkcie pobieranych przez element ObjectDataSource z klasy `ProductsBLL`.

Zacznij od otwarcia strony `ObjectDataSource.aspx` w folderze `Caching`. Przeciągnij widok GridView z przybornika do projektanta, ustaw jego właściwość `ID` na `Products`, a następnie z tagu inteligentnego Wybierz powiązanie go z nową kontrolką ObjectDataSource o nazwie `ProductsDataSource`. Skonfiguruj element ObjectDataSource do pracy z klasą `ProductsBLL`.

[![skonfigurować element ObjectDataSource do używania klasy ProductsBLL](caching-data-with-the-objectdatasource-cs/_static/image7.png)](caching-data-with-the-objectdatasource-cs/_static/image6.png)

**Ilustracja 4**. Konfigurowanie elementu ObjectDataSource do używania klasy `ProductsBLL` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](caching-data-with-the-objectdatasource-cs/_static/image8.png))

Na tej stronie poczekaj, aż utworzysz edytowalny widok GridView, aby można było ocenić, co się dzieje, gdy dane w pamięci podręcznej w elemencie ObjectDataSource są modyfikowane za pomocą interfejsu GridView. Pozostaw listę rozwijaną na karcie Wybierz ustawioną na domyślną, `GetProducts()`, ale Zmień wybrany element na karcie aktualizacja na wartość przeciążenia `UpdateProduct`, która akceptuje `productName`, `unitPrice`i `productID` jako parametry wejściowe.

[![ustawić listę rozwijaną kart Aktualizuj na odpowiednie Przeciążenie UpdateProduct](caching-data-with-the-objectdatasource-cs/_static/image10.png)](caching-data-with-the-objectdatasource-cs/_static/image9.png)

**Rysunek 5**. Ustawianie listy rozwijanej kart aktualizacji na odpowiednie Przeciążenie `UpdateProduct` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](caching-data-with-the-objectdatasource-cs/_static/image11.png))

Na koniec Ustaw listę rozwijaną na kartach Wstaw i Usuń na (brak), a następnie kliknij przycisk Zakończ. Po zakończeniu działania Kreatora konfiguracji źródła danych program Visual Studio ustawia właściwość `OldValuesParameterFormatString` ObjectDataSource s na `original_{0}`. Zgodnie z opisem w temacie [Omówienie wstawiania, aktualizowania i usuwania danych](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) , ta właściwość musi zostać usunięta ze składni deklaratywnej lub ustawiona z powrotem na wartość domyślną `{0}`, aby nasz przepływ pracy aktualizacji kontynuował pracę bez błędu.

Ponadto po zakończeniu pracy kreatora program Visual Studio dodaje pole do widoku GridView dla każdego z pól danych produktu. Usuń wszystkie oprócz `ProductName`, `CategoryName`i `UnitPrice` BoundFields. Następnie zaktualizuj właściwości `HeaderText` każdego z BoundFields odpowiednio do produktu, kategorii i ceny. Ponieważ pole `ProductName` jest wymagane, przekonwertuj BoundField na TemplateField i Dodaj RequiredFieldValidator do `EditItemTemplate`. Podobnie przekonwertuj `UnitPrice` BoundField na TemplateField i Dodaj CompareValidator, aby upewnić się, że wartość wprowadzona przez użytkownika jest prawidłową wartością waluty równą zero lub większą. Oprócz tych modyfikacji można korzystać z dowolnych zmian estetycznych, takich jak wyrównanie wartości `UnitPrice` lub określanie formatowania tekstu `UnitPrice` w interfejsach tylko do odczytu i edytowania.

Przekształć element GridView w edytowalny, zaznaczając pole wyboru Włącz edycję w tagu inteligentnym GridView. Zaznacz również pole wyboru Włącz stronicowanie i Włącz sortowanie.

> [!NOTE]
> Potrzebujesz przeglądu, jak dostosować interfejs edytowania GridView Jeśli tak, zapoznaj się z artykułem [Dostosowywanie interfejsu modyfikacji danych](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) .

[![włączyć obsługę widoku GridView do edycji, sortowania i stronicowania](caching-data-with-the-objectdatasource-cs/_static/image13.png)](caching-data-with-the-objectdatasource-cs/_static/image12.png)

**Ilustracja 6**. Włączanie obsługi widoku GridView do edycji, sortowania i stronicowania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](caching-data-with-the-objectdatasource-cs/_static/image14.png))

Po dokonaniu tych modyfikacji GridView, znaczniki deklaracyjne i ObjectDataSource nie powinny wyglądać podobnie do następujących:

[!code-aspx[Main](caching-data-with-the-objectdatasource-cs/samples/sample2.aspx)]

Jak pokazano na rysunku 7, edytowalny widok GridView wyświetla nazwę, kategorię i cenę każdego produktu w bazie danych. Poświęć chwilę na przetestowanie funkcji strony, Sortuj wyniki, przechodząc do nich stronę i edytuj rekord.

[![każdą nazwę produktu, kategorię i cenę są wyświetlane w obszarze Sortowanie, stronicowanie i edytowanie widoku GridView](caching-data-with-the-objectdatasource-cs/_static/image16.png)](caching-data-with-the-objectdatasource-cs/_static/image15.png)

**Rysunek 7**. Każda nazwa produktu, Kategoria i cena są wymienione w obszarze Sortowanie, stronicowanie i edytowanie widoku GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](caching-data-with-the-objectdatasource-cs/_static/image17.png))

## <a name="step-3-examining-when-the-objectdatasource-is-requesting-data"></a>Krok 3. Sprawdzanie, czy element ObjectDataSource żąda danych

`Products` GridView pobiera dane do wyświetlenia przez wywołanie metody `Select` `ProductsDataSource` ObjectDataSource. Ten element ObjectDataSource tworzy wystąpienie klasy logiki biznesowej `ProductsBLL` klasie i wywołuje metodę `GetProducts()`, która z kolei wywołuje metodę dostępu do danych s `ProductsTableAdapter` s `GetProducts()`. Metoda DAL nawiązuje połączenie z bazą danych Northwind i wystawia skonfigurowaną kwerendę `SELECT`. Te dane są następnie zwracane do DAL, które pakują je w `NorthwindDataTable`. Obiekt DataTable jest zwracany do LOGIKI biznesowej, który zwraca go do elementu ObjectDataSource, który zwraca go do widoku GridView. Następnie GridView tworzy obiekt `GridViewRow` dla każdego `DataRow` w elemencie DataTable, a każdy `GridViewRow` jest ostatecznie renderowany w kodzie HTML, który jest zwracany do klienta i wyświetlany w przeglądarce odwiedzających.

Ta sekwencja zdarzeń występuje każdego, a za każdym razem, gdy GridView musi powiązać dane bazowe. Dzieje się tak, gdy strona jest po raz pierwszy odwiedzana podczas przechodzenia z jednej strony danych do innej, podczas sortowania GridView lub modyfikowania danych GridView za pośrednictwem wbudowanej edycji lub usuwania interfejsów. Jeśli stan widoku GridView jest wyłączony, element GridView zostanie Przewiązany na każdym i każdym ogłaszaniu zwrotnym. Widok GridView można również jawnie powiązać ze swoimi danymi, wywołując metodę `DataBind()`.

Aby w pełni uzyskać częstotliwość, z jaką dane są pobierane z bazy danych, pozwól na wyświetlenie komunikatu wskazującego, kiedy dane są pobierane ponownie. Dodaj kontrolkę sieci Web etykieta powyżej widoku GridView o nazwie `ODSEvents`. Wyczyść Właściwość `Text` i ustaw jej Właściwość `EnableViewState` na `false`. Poniżej etykiety Dodaj kontrolkę sieci Web przycisk i ustaw jej Właściwość `Text` na zwrotne.

[![dodać etykietę i przycisk do strony powyżej widoku GridView](caching-data-with-the-objectdatasource-cs/_static/image19.png)](caching-data-with-the-objectdatasource-cs/_static/image18.png)

**Ilustracja 8**. Dodawanie etykiety i przycisku do strony nad widokiem GridView ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](caching-data-with-the-objectdatasource-cs/_static/image20.png))

Podczas przepływu pracy dostępu do danych zdarzenie ObjectDataSource s `Selecting` generowane przed utworzeniem obiektu źródłowego i wywołaniem jego skonfigurowanej metody. Utwórz procedurę obsługi zdarzeń dla tego zdarzenia i Dodaj następujący kod:

[!code-csharp[Main](caching-data-with-the-objectdatasource-cs/samples/sample3.cs)]

Za każdym razem, gdy element ObjectDataSource wykonuje żądanie do architektury danych, w etykiecie zostanie wyświetlone zdarzenie wybierania tekstu.

Odwiedź Tę stronę w przeglądarce. Po pierwszym odwiedzeniu strony zostanie wyświetlone zdarzenie z wybieraniem tekstu. Kliknij przycisk ogłaszania zwrotnego i Zauważ, że tekst znika (przy założeniu, że właściwość GridView `EnableViewState` jest ustawiona na `true`, wartość domyślna). Jest to spowodowane tym, że w przypadku ogłaszania wstecznego widok GridView jest ponownie skonstruowany ze stanu widoku, dlatego nie jest on przełączany do elementu ObjectDataSource dla swoich danych. Jednak sortowanie, stronicowanie lub edytowanie danych powoduje, że widok GridView zostanie ponownie powiązany ze źródłem danych, a w związku z tym zostanie wyświetlony tekst wygenerowanego zdarzenia.

[![zawsze, gdy widok GridView zostanie odwiązany ze źródłem danych, zostanie wyświetlona opcja Wyzwalanie zdarzeń](caching-data-with-the-objectdatasource-cs/_static/image22.png)](caching-data-with-the-objectdatasource-cs/_static/image21.png)

**Rysunek 9**. zawsze, gdy widok GridView zostanie Przewiązany ze źródłem danych, zostanie wyświetlona opcja wygenerowane zdarzenie ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](caching-data-with-the-objectdatasource-cs/_static/image23.png))

[![kliknięcie przycisku ogłaszania zwrotnego powoduje odbudowanie widoku GridView ze stanu widoku](caching-data-with-the-objectdatasource-cs/_static/image25.png)](caching-data-with-the-objectdatasource-cs/_static/image24.png)

**Ilustracja 10**. kliknięcie przycisku ogłaszania zwrotnego powoduje odtworzenie widoku GridView ze stanu widoku ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](caching-data-with-the-objectdatasource-cs/_static/image26.png))

Może się wydawać, że wasteful pobrać dane bazy danych za każdym razem, gdy dane są stronicowane lub sortowane. Ze względu na to, że od momentu ponownego użycia domyślnego stronicowania, element ObjectDataSource pobrał wszystkie rekordy podczas wyświetlania pierwszej strony. Nawet jeśli GridView nie zapewnia obsługi sortowania i stronicowania, dane muszą zostać pobrane z bazy danych za każdym razem, gdy strona jest najpierw odwiedzana przez dowolnego użytkownika (i na każdym odświeżeniu, jeśli stan widoku jest wyłączony). Ale jeśli GridView pokazuje te same dane dla wszystkich użytkowników, te dodatkowe żądania bazy danych są zbędne. Dlaczego nie można buforować wyników zwróconych z metody `GetProducts()` i powiązać widoku GridView z tymi, które są w nich buforowane?

## <a name="step-4-caching-the-data-using-the-objectdatasource"></a>Krok 4. buforowanie danych przy użyciu elementu ObjectDataSource

Po prostu ustawiając kilka właściwości, element ObjectDataSource można skonfigurować w taki sposób, aby automatycznie buforuł pobrane dane w pamięci podręcznej danych ASP.NET. Poniższa lista zawiera podsumowanie właściwości elementów ObjectDataSource związanych z pamięcią podręczną:

- Aby włączyć buforowanie, [EnableCaching](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.enablecaching.aspx) musi być ustawiona na wartość `true`. Wartość domyślna to `false`.
- [CacheDuration](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheduration.aspx) ilość czasu (w sekundach), przez który dane są buforowane. Wartość domyślna to 0. Element ObjectDataSource będzie buforować dane tylko wtedy, gdy `EnableCaching` jest `true` i `CacheDuration` jest ustawiona na wartość większą od zera.
- [CacheExpirationPolicy](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheexpirationpolicy.aspx) można ustawić na `Absolute` lub `Sliding`. W przypadku `Absolute`element ObjectDataSource zapisuje w pamięci podręcznej pobrane dane przez `CacheDuration` sekund; w przypadku `Sliding`dane wygasną dopiero po upływie `CacheDuration` sekund. Wartość domyślna to `Absolute`.
- [CacheKeyDependency](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cachekeydependency.aspx) ta właściwość służy do kojarzenia wpisów pamięci podręcznej ObjectDataSource s z istniejącą zależnością pamięci podręcznej. Wpisy danych ObjectDataSource s można przedwcześnie wykluczyć z pamięci podręcznej, tracąc skojarzone `CacheKeyDependency`. Ta właściwość jest najczęściej używana do kojarzenia zależności pamięci podręcznej SQL z pamięcią podręczną programu ObjectDataSources, a temat zostanie wyświetlony w przyszłości [przy użyciu samouczka zależności pamięci podręcznej SQL](using-sql-cache-dependencies-cs.md) .

Zezwól usłudze s na skonfigurowanie `ProductsDataSource` elementu ObjectDataSource do buforowania danych przez 30 sekund na bezwzględnej skali. Ustaw właściwość `EnableCaching` ObjectDataSource s na `true` i jej Właściwość `CacheDuration` na wartość 30. Pozostaw domyślną właściwość `CacheExpirationPolicy`, `Absolute`.

[![skonfigurować element ObjectDataSource do buforowania danych przez 30 sekund](caching-data-with-the-objectdatasource-cs/_static/image28.png)](caching-data-with-the-objectdatasource-cs/_static/image27.png)

**Ilustracja 11**. Konfigurowanie elementu ObjectDataSource do buforowania danych przez 30 sekund ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](caching-data-with-the-objectdatasource-cs/_static/image29.png))

Zapisz zmiany i ponownie odwiedź tę stronę w przeglądarce. Tekst, który zostanie wywołany, zostanie wyświetlony podczas pierwszej wizyty strony, jak początkowo dane nie znajdują się w pamięci podręcznej. Ale kolejne ogłaszanie zwrotne wyzwalane przez kliknięcie przycisku ogłaszania zwrotnego, sortowanie, stronicowanie lub kliknięcie przycisków Edytuj lub Anuluj *nie powoduje ponowne* wyświetlenia tekstu uruchomienia zdarzenia. Wynika to z faktu, że zdarzenie `Selecting` wyzwalane tylko wtedy, gdy element ObjectDataSource pobiera swoje dane z obiektu źródłowego; zdarzenie `Selecting` nie zostanie wywołane, jeśli dane są pobierane z pamięci podręcznej danych.

Po 30 sekundach dane zostaną wykluczone z pamięci podręcznej. Dane zostaną również wykluczone z pamięci podręcznej, jeśli są wywoływane metody `Insert`, `Update`lub `Delete` elementu ObjectDataSource. W związku z tym po upływie 30 sekund lub kliknięciu przycisku Aktualizuj, sortowaniu, stronicowaniu lub kliknięciu przycisków Edytuj lub Anuluj spowoduje to, że element ObjectDataSource ma pobrać dane z obiektu bazowego, co spowoduje wyświetlenie tekstu wywoływanego po zdarzeniu `Selecting`. Zwrócone wyniki są umieszczane w pamięci podręcznej danych.

> [!NOTE]
> Jeśli widzisz częste zdarzenie wybierania tekstu, nawet jeśli oczekujesz, że element ObjectDataSource ma pracować z danymi buforowanymi, może to być spowodowane ograniczeniami pamięci. Jeśli nie ma wystarczającej ilości wolnej pamięci, dane dodane do pamięci podręcznej przez element ObjectDataSource mogły zostać oczyszczone. Jeśli element ObjectDataSource nie będzie prawidłowo buforujący danych lub tylko buforuje dane, Zamknij niektóre aplikacje, aby zwolnić pamięć, i spróbuj ponownie.

Rysunek 12 ilustruje przepływ pracy buforowania elementu ObjectDataSource. Gdy na ekranie pojawi się tekst wywoływanego zdarzenia wyboru, jest to spowodowane tym, że dane nie były w pamięci podręcznej i musiały zostać pobrane z obiektu źródłowego. Gdy ten tekst nie zostanie utracony, ponieważ dane były dostępne z pamięci podręcznej. Gdy dane są zwracane z pamięci podręcznej, nie są wywoływane z obiektem źródłowym, dlatego nie wykonano zapytania do bazy danych.

![Element ObjectDataSource przechowuje i pobiera dane z pamięci podręcznej danych](caching-data-with-the-objectdatasource-cs/_static/image30.png)

**Ilustracja 12**. Element ObjectDataSource przechowuje i pobiera dane z pamięci podręcznej danych

Każda aplikacja ASP.NET ma swoje własne wystąpienie pamięci podręcznej danych, które jest udostępniane na wszystkich stronach i odwiedzających. Oznacza to, że dane przechowywane w pamięci podręcznej danych przez element ObjectDataSource są również współużytkowane przez wszystkich użytkowników, którzy odwiedzają stronę. Aby to sprawdzić, Otwórz stronę `ObjectDataSource.aspx` w przeglądarce. Podczas pierwszego odwiedzania strony zostanie wyświetlony tekst, który zostanie wywołany podczas wybierania zdarzenia (przy założeniu, że dane dodane do pamięci podręcznej przez poprzednie testy mają teraz wykluczone). Otwórz drugie wystąpienie przeglądarki i skopiuj i wklej adres URL z pierwszego wystąpienia przeglądarki do drugiego. W drugim wystąpieniu przeglądarki tekst, który został wywołany, nie jest wyświetlany, ponieważ s używa tych samych danych w pamięci podręcznej co pierwszy.

Podczas wstawiania pobranych danych do pamięci podręcznej element ObjectDataSource używa wartości klucza pamięci podręcznej, która zawiera: wartości właściwości `CacheDuration` i `CacheExpirationPolicy`; Typ bazowego obiektu biznesowego używany przez element ObjectDataSource, który jest określany za pomocą [właściwości`TypeName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.typename.aspx) (`ProductsBLL`, w tym przykładzie); wartość właściwości `SelectMethod` i nazwy i wartości parametrów w kolekcji `SelectParameters`; i wartości właściwości `StartRowIndex` i `MaximumRows`, które są używane podczas implementowania [stronicowania niestandardowego.](../paging-and-sorting/paging-and-sorting-report-data-cs.md)

Ponosząc wartość klucza pamięci podręcznej jako kombinację tych właściwości, zapewnia unikatowy wpis pamięci podręcznej w miarę zmiany tych wartości. Na przykład w poprzednich samouczkach, które zostały przeszukane przy użyciu `GetProductsByCategoryID(categoryID)`klasy `ProductsBLL`, która zwraca wszystkie produkty z określonej kategorii. Jeden użytkownik może przechodzić do strony i wyświetlać napoje, które mają `CategoryID` 1. Jeśli element ObjectDataSource w pamięci podręcznej nie ma wpływu na wartości `SelectParameters`, gdy inny użytkownik przeprowadził do strony, aby wyświetlić przyprawy, podczas gdy produkty napoje znajdowały się w pamięci podręcznej, widzą produkty z pamięci podręcznej napoje zamiast przypraw. Przez zróżnicowanie klucza pamięci podręcznej przez te właściwości, które obejmują wartości `SelectParameters`, element ObjectDataSource utrzymuje oddzielny wpis pamięci podręcznej dla napojów i przypraw.

## <a name="stale-data-concerns"></a>Nieodświeżone zagadnienia dotyczące danych

Element ObjectDataSource automatycznie wyklucza jego elementy z pamięci podręcznej w przypadku wywołania jednej z jej metod `Insert`, `Update`lub `Delete`. Pomaga to chronić przed nieodświeżonymi danymi przez wyczyszczenie wpisów pamięci podręcznej, gdy dane są modyfikowane za pomocą strony. Jednak jest możliwe, że element ObjectDataSource używający buforowania, aby nadal wyświetlał nieodświeżone dane. W najprostszym przypadku może to być spowodowane zmianą danych bezpośrednio w bazie danych. Być może administrator bazy danych właśnie uruchomił skrypt, który modyfikuje niektóre rekordy w bazie danych.

Ten scenariusz może również unfold w bardziej subtelny sposób. Gdy element ObjectDataSource wyklucza swoje elementy z pamięci podręcznej, gdy zostanie wywołana jedna z metod modyfikacji danych, elementy w pamięci podręcznej są usuwane dla kombinacji wartości właściwości (`CacheDuration`, `TypeName`, `SelectMethod`itd.). Jeśli istnieją dwa elementy ObjectDataSource, które używają różnych `SelectMethods` lub `SelectParameters`, ale nadal mogą aktualizować te same dane, jeden element ObjectDataSource może zaktualizować wiersz i unieważniać jego własne wpisy w pamięci podręcznej, ale odpowiedni wiersz dla drugiego elementu ObjectDataSource nadal będzie obsługiwany z pamięci podręcznej. Zachęcam do tworzenia stron w celu wystawiania tej funkcji. Utwórz stronę wyświetlającą edytowalny widok GridView, który pobiera dane z elementu ObjectDataSource, który używa buforowania i jest skonfigurowany do pobierania danych z metody `GetProducts()` `ProductsBLL` Class. Dodaj kolejny edytowalny element GridView i element ObjectDataSource do tej strony (lub innego), ale dla tego drugiego elementu ObjectDataSource Użyj metody `GetProductsByCategoryID(categoryID)`. Ponieważ dwa Właściwości ObjectDataSource `SelectMethod` różnią się, wszystkie one mają swoje własne wartości pamięci podręcznej. Jeśli edytujesz produkt w jednej siatce, następnym razem, gdy powiążesz dane z powrotem z inną siatką (według stronicowania, sortowania itd.), nadal będzie on obsługiwał stare, buforowane dane i nie odzwierciedlają zmiany wprowadzonej z drugiej siatki.

W skrócie Wykorzystaj tylko wygaśnięcia czasu, jeśli wolisz mieć potencjalną liczbę starych danych i używasz krótszych wygaśnięcia dla scenariuszy, w których aktualność danych jest ważna. Jeśli stare dane nie są akceptowalne, buforowanie forgo lub użycie zależności pamięci podręcznej SQL (przy założeniu, że dane są ponownie buforowane). Zapoznajemy zależności pamięci podręcznej SQL w przyszłym samouczku.

## <a name="summary"></a>Podsumowanie

W tym samouczku zbadano wbudowane możliwości buforowania elementu ObjectDataSource. Po prostu ustawiając kilka właściwości, możemy nakazać elementowi ObjectDataSource Buforowanie wyników zwróconych z określonego `SelectMethod` do pamięci podręcznej danych ASP.NET. Właściwości `CacheDuration` i `CacheExpirationPolicy` wskazują czas trwania przechowywania elementu w pamięci podręcznej oraz to, czy jest to bezwzględne czy ruchome. Właściwość `CacheKeyDependency` kojarzy wszystkie wpisy pamięci podręcznej ObjectDataSource s z istniejącą zależnością pamięci podręcznej. Można go użyć do wykluczenia wpisów ObjectDataSource s z pamięci podręcznej przed osiągnięciem wygaśnięcia na podstawie czasu i jest zazwyczaj używany z zależnościami pamięci podręcznej SQL.

Ponieważ element ObjectDataSource po prostu zapisuje swoje wartości w pamięci podręcznej danych, można programowo replikować wbudowane funkcje programu ObjectDataSource. Nie ma sensu t należy to zrobić w warstwie prezentacji, ponieważ element ObjectDataSource oferuje tę funkcję, ale można zaimplementować możliwości buforowania w oddzielnej warstwie architektury. Aby to zrobić, należy powtórzyć tę samą logikę używaną przez element ObjectDataSource. Dowiesz się, jak programowo współpracować z pamięcią podręczną danych z poziomu architektury w naszym następnym samouczku.

Szczęśliwe programowanie!

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Buforowanie ASP.NET: techniki i najlepsze rozwiązania](https://msdn.microsoft.com/library/aa478965.aspx)
- [Przewodnik po architekturze buforowania dla aplikacji .NET Framework](https://msdn.microsoft.com/library/ee817645.aspx)
- [Buforowanie danych wyjściowych w ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzent potencjalnych klientów dla tego samouczka został Teresa Murphy. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](caching-data-in-the-architecture-cs.md)

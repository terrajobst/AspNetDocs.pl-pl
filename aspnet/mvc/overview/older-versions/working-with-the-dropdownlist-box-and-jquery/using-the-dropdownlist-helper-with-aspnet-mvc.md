---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
title: Korzystanie z pomocnika DropDownList z ASP.NET MVC | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 53767e05-c8ab-42e1-a94b-22d906195200
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 0ded9fea8a77824645e87c37cdb3376e618a2f25
ms.sourcegitcommit: e365196c75ce93cd8967412b1cfdc27121816110
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/07/2020
ms.locfileid: "77075128"
---
# <a name="using-the-dropdownlist-helper-with-aspnet-mvc"></a>Używanie pomocnika DropDownList we wzorcu ASP.NET MVC

Autor [Rick Anderson]((https://twitter.com/RickAndMSFT))

W tym samouczku przedstawiono podstawowe informacje dotyczące pracy z pomocnikiem [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) i pomocnika [ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) w aplikacji sieci Web ASP.NET MVC. Możesz użyć programu Microsoft Visual Web Developer 2010 Express z dodatkiem Service Pack 1, który jest bezpłatną wersją Microsoft Visual Studio, aby postępować zgodnie z samouczkiem. Przed rozpoczęciem upewnij się, że zainstalowano wymagania wstępne wymienione poniżej. Wszystkie z nich można zainstalować, klikając następujące łącze: [Instalator platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie możesz zainstalować wstępnie wymagane składniki, korzystając z następujących linków:

- [Wymagania wstępne programu Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack) <a id="post"></a>
- [Aktualizacja narzędzi ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(obsługa środowiska uruchomieniowego + narzędzia)

Jeśli używasz programu Visual Studio 2010 zamiast programu Visual Web Developer 2010, Zainstaluj wymagania wstępne, klikając następujące łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack). W tym samouczku przyjęto założenie, że zakończono [wprowadzenie do samouczka ASP.NET MVC](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub samouczka[ASP.NET MVC Music](../mvc-music-store/mvc-music-store-part-1.md) Ten samouczek rozpoczyna się od zmodyfikowanego projektu z samouczka dotyczącego [sklepu ASP.NET MVC Music](../mvc-music-store/mvc-music-store-part-1.md) . Możesz pobrać projekt początkowy z następującym linkiem, aby [pobrać C# wersję](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829).

Projekt Visual Web Developer z kompletnym kodem źródłowym C# samouczka jest dostępny do tego tematu. [Pobierz](https://code.msdn.microsoft.com/Using-the-DropDownList-67f9367d).

### <a name="what-youll-build"></a>Co będziesz kompilować

Utworzysz metody akcji i widoki, które używają pomocnika [DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) , aby wybrać kategorię. Za pomocą programu **jQuery** można także dodać okno dialogowe Wstaw kategorię, które może być używane, gdy jest wymagana Nowa kategoria (na przykład gatunek lub wykonawca). Poniżej znajduje się zrzut ekranu przedstawiający widok tworzenie, który zawiera linki umożliwiające dodanie nowego gatunku i dodanie nowego wykonawcy.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image1.png)

### <a name="skills-youll-learn"></a>Posiadane umiejętności

Oto, co uzyskasz:

- Jak używać pomocnika [DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) do wybierania danych kategorii.
- Jak dodać nowe kategorie przy użyciu okna dialogowego **jQuery** .

### <a name="getting-started"></a>Getting Started

Zacznij od pobrania początkowego projektu z następującym łączem, [Pobierz](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829). W Eksploratorze Windows kliknij prawym przyciskiem myszy plik *DDL\_Starter. zip* , a następnie wybierz polecenie Właściwości. W oknie dialogowym **właściwości języka DDL\_Starter. zip** wybierz opcję Odblokuj.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image2.png)

Kliknij prawym przyciskiem myszy plik DDL\_Starter. zip, a następnie wybierz pozycję **Wyodrębnij wszystko** , aby rozpakować plik. Otwórz plik *StartMusicStore. sln* w programie Visual web developer 2010 Express ("Visual Web Developer" lub "pliku VWD" for Short) lub Visual Studio 2010.

Naciśnij klawisze CTRL + F5, aby uruchomić aplikację, a następnie kliknij link **testowy** .

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image3.png)

Wybierz łącze **Wybierz kategorię filmu (prosty)** . Zostanie wyświetlona lista wyboru typu filmu z komedia wybraną wartością.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image4.png)

Kliknij prawym przyciskiem myszy w przeglądarce i wybierz polecenie Wyświetl źródło. Zostanie wyświetlony kod HTML strony. Poniższy kod pokazuje kod HTML dla elementu Select.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample1.html)]

Można zobaczyć, że każdy element na liście wyboru ma wartość (0 dla akcji, 1 dla dramat, 2 dla komedia i 3 dla fikcyjnej nauki) i nazwę wyświetlaną (Action, dramat, komedia i fikcja). Powyżej znajduje się kod standardowy HTML dla listy wyboru.

Zmień listę wyboru na dramat i naciśnij przycisk **Prześlij** . Adres URL w przeglądarce jest `http://localhost:2468/Home/CategoryChosen?MovieType=1` i zostanie wyświetlona strona **: 1**.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image5.png)

Otwórz plik *Controllers\HomeController.cs* i Przeanalizuj metodę `SelectCategory`.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample2.cs)]

Pomocnik [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) użyty do utworzenia listy wyboru HTML wymaga, aby **interfejs IEnumerable&lt;SelectListItem &gt;** , jawnie lub niejawnie. Oznacza to, że można przekazać **interfejs ienumerable&lt;SelectListItem &gt;** jawnie do pomocnika [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) lub dodać **&gt;interfejsu IEnumerable&lt;SelectListItem** do [ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) przy użyciu tej samej nazwy dla **SelectListItem** jako właściwości modelu. Przekazywanie w **SelectListItem** niejawnie i jawnie jest omówione w następnej części samouczka. W powyższym kodzie przedstawiono najprostszy możliwy sposób tworzenia **interfejsu IEnumerable&lt;SelectListItem &gt;** i wypełnienia go tekstem i wartościami. Zwróć uwagę na [to, że](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.selected.aspx) `Comedy`[SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) ma ustawioną **wartość true.** spowoduje to, że renderowanej listy wyboru pokaże **komedia** jako wybrany element na liście.

Utworzone powyżej **&gt;IEnumerable&lt;SelectListItem** są dodawane do [ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) z nazwą movietype. Jest to sposób, w jaki przekazujemy **interfejs IEnumerable&lt;SelectListItem &gt;** niejawnie do pomocnika [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) pokazanego poniżej.

Otwórz plik *Views\Home\SelectCategory.cshtml* i zapoznaj się z adiustacją.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample3.cshtml)]

W trzecim wierszu ustawimy układ na widoki/Shared/\_prostego\_Layout. cshtml, który jest uproszczoną wersją standardowego pliku układu. W ten sposób można zachować prostotę wyświetlania i renderowanego kodu HTML.

W tym przykładzie nie zmieniamy stanu aplikacji, więc wyślemy dane przy użyciu polecenia **http Get**, a nie **http post**. Zapoznaj się z sekcją W3C z [szybką listą kontrolną dotyczącą wybierania HTTP GET lub post](http://www.w3.org/2001/tag/doc/whenToUseGet.html#checklist). Ponieważ nie zmieniamy aplikacji i nie ogłaszamy formularza, używamy przeciążenia [HTML. BeginForm](https://msdn.microsoft.com/library/dd460344.aspx) , które umożliwia określenie metody akcji, kontrolera i metody formularza (**http post** lub **http Get**). Zazwyczaj widoki zawierają Przeciążenie [HTML. BeginForm](https://msdn.microsoft.com/library/dd505244.aspx) , które nie przyjmuje żadnych parametrów. Wersja parametru nie jest wartością domyślną do publikowania danych formularza w wersji POST tej samej metody i kontrolera akcji.

Poniższy wiersz

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample4.cshtml)]

przekazuje argument ciągu do pomocnika **DropDownList** . Ten ciąg "Movietype" w naszym przykładzie ma dwie rzeczy:

- Udostępnia klucz pomocnika **DropDownList** , aby znaleźć **interfejs IEnumerable&lt;SelectListItem &gt;** w **ViewBag**.
- Jest on powiązany z danymi do elementu form. Jeśli metoda przesyłania to **http Get**, `MovieType` będzie ciągiem zapytania. Jeśli metoda przesyłania jest **wpisem http post**, `MovieType` zostanie dodana w treści komunikatu. Na poniższej ilustracji przedstawiono ciąg zapytania z wartością 1.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image6.png)

Poniższy kod przedstawia metodę `CategoryChosen`, do której formularz został przesłany.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample5.cs)]

Przejdź z powrotem do strony test i wybierz łącze **HTML SelectList** . Strona HTML renderuje element SELECT podobny do strony testowej ASP.NET MVC. Kliknij prawym przyciskiem myszy okno przeglądarki i wybierz polecenie **Wyświetl źródło**. Znaczniki HTML dla listy wyboru są zasadniczo identyczne. Przetestuj stronę HTML, która działa jak Metoda akcji ASP.NET MVC i sprawdź, że wcześniej przetestowano.

### <a name="improving-the-movie-select-list-with-enums"></a>Ulepszanie listy wyboru filmu z wyliczeniami

Jeśli kategorie w aplikacji są naprawione i nie zmienią się, można skorzystać z wyliczeń, aby kod był bardziej niezawodny i łatwiejszy do rozbudowania. Po dodaniu nowej kategorii jest generowana poprawna wartość kategorii. Pozwala to uniknąć błędów kopiowania i wklejania po dodaniu nowej kategorii, ale zapomnieniu o zaktualizowaniu wartości kategorii.

Otwórz plik *Controllers\HomeController.cs* i Przeanalizuj następujący kod:

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample6.cs)]

[Wyliczenie](https://msdn.microsoft.com/library/sbbt4032(VS.80).aspx) `eMovieCategories` przechwytuje cztery typy filmów. Metoda `SetViewBagMovieType` tworzy **&gt;IEnumerable&lt;SelectListItem** z **wyliczenia**`eMovieCategories`i ustawia właściwość `Selected` z parametru `selectedMovie`. Metoda akcji `SelectCategoryEnum` używa tego samego widoku co Metoda akcji `SelectCategory`.

Przejdź do strony test i kliknij łącze `Select Movie Category (Enum)`. Tym razem, zamiast wyświetlanej wartości (liczby), wyświetlany jest ciąg reprezentujący Wyliczenie.

### <a name="posting-enum-values"></a>Księgowanie wartości wyliczeniowych

Formularze HTML są zwykle używane do publikowania danych na serwerze. Poniższy kod przedstawia `HTTP GET` i `HTTP POST` wersje metody `SelectCategoryEnumPost`.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample7.cs)]

Przekazując `eMovieCategories` Wyliczenie do metody `POST`, możemy wyodrębnić zarówno wartość enum, jak i ciąg wyliczeniowy. Uruchom przykład i przejdź do strony test. Kliknij link `Select Movie Category(Enum Post)`. Wybierz typ filmu, a następnie naciśnij przycisk Prześlij. Wyświetla wartość i nazwę typu filmu.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image7.png)

### <a name="creating-a-multiple-section-select-element"></a>Tworzenie elementu Select z wieloma sekcjami

Pomocnik HTML [ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) renderuje element `<select>` HTML z atrybutem `multiple`, dzięki czemu użytkownicy mogą wybrać wiele zaznaczeń. Przejdź do linku test, a następnie wybierz łącze wybór **kraju wielokrotnego** . Renderowany interfejs użytkownika umożliwia wybranie wielu krajów. Na poniższej ilustracji wybrano Kanadę i Chiny.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image8.png)

### <a name="examining-the-multiselectcountry-code"></a>Badanie kodu MultiSelectCountry

Przejrzyj następujący kod z pliku *Controllers\HomeController.cs* .

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample8.cs)]

Metoda `GetCountries` tworzy listę krajów, a następnie przekazuje ją do konstruktora `MultiSelectList`. Przeciążenie konstruktora `MultiSelectList` użyte w powyższej metodzie `GetCountries` przyjmuje cztery parametry:

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample9.cs)]

1. *elementy*: element [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) zawierający elementy z listy. W powyższym przykładzie lista krajów.
2. *dataValueField*: Nazwa właściwości na liście **IEnumerable** , która zawiera wartość. W powyższym przykładzie właściwość `ID`.
3. *dataTextField*: Nazwa właściwości na liście **IEnumerable** , która zawiera informacje do wyświetlenia. W powyższym przykładzie właściwość `name`.
4. *selectedValues*: Lista wybranych wartości.

W powyższym przykładzie metoda `MultiSelectCountry` przekazuje wartość `null` dla wybranych krajów, więc nie są wybierane żadne kraje podczas wyświetlania interfejsu użytkownika. Poniższy kod przedstawia znacznik Razor używany do renderowania widoku `MultiSelectCountry`.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample10.cshtml)]

Metoda [pola](https://msdn.microsoft.com/library/dd470200.aspx) pomocnika HTML użyta powyżej przyjmuje dwa parametry, nazwę właściwości powiązania modelu i [MultiSelectList](https://msdn.microsoft.com/library/system.web.mvc.multiselectlist.aspx) zawierający opcje i wartości. Powyższy kod `ViewBag.YouSelected` służy do wyświetlania wartości krajów wybranych podczas przesyłania formularza. Sprawdzanie przeciążenia POST protokołu HTTP metody `MultiSelectCountry`.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample11.cs)]

Właściwość dynamiczna `ViewBag.YouSelected` zawiera wybrane kraje uzyskane dla wpisu `Countries` w kolekcji formularzy. W tej wersji Metoda getkrajach jest przenoszona na listę wybranych krajów, więc po wyświetleniu widoku `MultiSelectCountry` wybrane kraje są wybrane w interfejsie użytkownika.

### <a name="making-a-select-element-friendly-with-the-harvest-chosen-jquery-plugin"></a>Uczynienie elementu Select przyjaznym dla wtyczki z wybranym zbiorem jQuery

[Wybraną](https://harvesthq.github.com/chosen/) wtyczkę programu jQuery można dodać do pliku HTML &lt;wybierz element&gt;, aby utworzyć przyjazny interfejs użytkownika. Poniższe obrazy przedstawiają wtyczkę [wybranej](https://harvesthq.github.com/chosen/) platformy jQuery z widokiem `MultiSelectCountry`.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image9.png)

W poniższych dwóch obrazach jest wybrane **Kanada** .

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image10.png)

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image11.png)

Na powyższym obrazie Kanada jest zaznaczona i zawiera **znak x** , który można kliknąć, aby usunąć zaznaczenie. Na poniższej ilustracji przedstawiono wybrane dla Kanady, Chin i Japonii.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image12.png)

### <a name="hooking-up-the-harvest-chosen-jquery-plugin"></a>Podłączanie wybranej wtyczki jQuery

Jeśli masz pewne doświadczenie w korzystaniu z platformy jQuery, łatwiej jest skorzystać z poniższej sekcji. Jeśli wcześniej nie korzystano z platformy jQuery, możesz spróbować skorzystać z jednego z poniższych samouczków jQuery.

- [Jak działa działanie jQuery](http://docs.jquery.com/Tutorials:How_jQuery_Works) przez [Jan Resig](http://ejohn.org/)
- [Wprowadzenie z użyciem platformy jQuery](http://docs.jquery.com/Tutorials:Getting_Started_with_jQuery) przez [Jörn Zaefferer](http://bassistance.de/)
- [Przykłady na żywo platformy jQuery](http://codylindley.com/blogstuff/js/jquery/#) według [Cody Lindley](http://codylindley.com/)

Wybrana wtyczka jest zawarta w początkowym i zakończonym przykładowym projekcie, które towarzyszą w tym samouczku. W tym samouczku wystarczy użyć platformy jQuery, aby podłączyć ją do interfejsu użytkownika. Aby można było używać zbioru wybranych jQuery w projekcie ASP.NET MVC, należy:

1. Pobierz wybraną wtyczkę z usługi [GitHub](https://github.com/harvesthq/chosen/). Ten krok został wykonany.
2. Dodaj wybrany folder do projektu MVC ASP.NET. Dodaj zasoby z wybranej wtyczki pobranej w poprzednim kroku do wybranego folderu. Ten krok został wykonany.
3. Podłącz wybraną wtyczkę do pomocnika HTML **DropDownList** lub **ListBox** .

### <a name="hooking-up-the-chosen-plugin-to-the-multiselectcountry-view"></a>Podłączanie wybranej wtyczki do widoku MultiSelectCountry.

Otwórz plik *Views\Home\MultiSelectCountry.cshtml* i dodaj do `Html.ListBox`parametr `htmlAttributes`. Parametr, który zostanie dodany, zawiera nazwę klasy dla listy wyboru (`@class = "chzn-select"`). Ukończony kod jest przedstawiony poniżej:

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample12.cshtml)]

W powyższym kodzie dodajemy atrybut HTML i wartość atrybutu `class = "chzn-select"`. Znak \@ poprzedzający klasę nie ma nic do zrobienia w przypadku aparatu widoku Razor. `class` jest [ C# słowem kluczowym](https://msdn.microsoft.com/library/x53a06bb.aspx). C#Słowa kluczowe nie mogą być używane jako identyfikatory, chyba że zawierają \@ jako prefiks. W powyższym przykładzie `@class` jest prawidłowym identyfikatorem, ale **Klasa** nie jest, ponieważ **Klasa** jest słowem kluczowym.

Dodaj odwołania do *wybranego/wybranego pliku. jQuery. js* oraz *wybranych/wybranych plików CSS* . *Wybrany/wybrany. jQuery. js* i implementuje funkcjonalną funkcję wybranej wtyczki. *Wybrany/wybrany plik CSS* zawiera style. Dodaj te odwołania do dolnej części pliku *Views\Home\MultiSelectCountry.cshtml* . Poniższy kod pokazuje, jak odwołać się do wybranej wtyczki.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample13.cshtml)]

Aktywuj wybraną wtyczkę przy użyciu nazwy klasy używanej w kodzie **HTML. ListBox** . W powyższym przykładzie nazwa klasy jest `chzn-select`. Dodaj następujący wiersz w dolnej części pliku widoku *Views\Home\MultiSelectCountry.cshtml* . Ten wiersz aktywuje wybraną wtyczkę.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample14.html)]

Następujący wiersz jest składnią do wywołania funkcji gotowej jQuery, która wybiera Element DOM z nazwą klasy `chzn-select`.

[!code-powershell[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample15.ps1)]

Zestaw opakowany zwrócony z powyższego wywołania stosuje wybraną metodę (`.chosen();`), która przechwytuje wybraną wtyczkę.

Poniższy kod przedstawia ukończony plik widoku *Views\Home\MultiSelectCountry.cshtml* .

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample16.cshtml)]

Uruchom aplikację i przejdź do widoku `MultiSelectCountry`. Spróbuj dodać i usunąć kraje. Udostępnione pobranie próbek zawiera również metodę `MultiCountryVM` i widok implementujący funkcje MultiSelectCountry przy użyciu modelu widoku zamiast **ViewBag**.

W następnej sekcji zobaczysz, jak mechanizm tworzenia szkieletu ASP.NET MVC współpracuje z pomocnikiem **DropDownList** .

> [!div class="step-by-step"]
> [Dalej](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)

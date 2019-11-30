---
uid: web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-cs
title: Wyświetlanie danych binarnych w kontrolkach sieci WebC#danych () | Microsoft Docs
author: rick-anderson
description: W tym samouczku przedstawiono opcje przedstawiające dane binarne na stronie sieci Web, w tym wyświetlanie pliku obrazu i udostępnianie linku "Pobierz"...
ms.author: riande
ms.date: 03/27/2007
ms.assetid: 5cbeb9f8-5f92-4ba8-87ae-0b4d460ae6d4
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: f38de7adcd77b3dc2622759646168cf533b8308f
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74642532"
---
# <a name="displaying-binary-data-in-the-data-web-controls-c"></a>Wyświetlanie danych binarnych w kontrolkach internetowych danych (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_55_CS.exe) lub [Pobierz plik PDF](displaying-binary-data-in-the-data-web-controls-cs/_static/datatutorial55cs1.pdf)

> W tym samouczku przedstawiono opcje przedstawiające dane binarne na stronie sieci Web, w tym wyświetlanie pliku obrazu i udostępnianie linku "Pobieranie" dla pliku PDF.

## <a name="introduction"></a>Wprowadzenie

W powyższym samouczku przedstawiono dwie techniki kojarzenia danych binarnych z modelem danych programu Application s i użycie kontrolki FileUpload do przekazywania plików z przeglądarki do systemu plików serwera sieci Web. Jeszcze teraz widzimy, jak skojarzyć przekazane dane binarne z modelem danych. Oznacza to, że po przekazaniu pliku i zapisaniu go w systemie plików ścieżka do pliku musi być przechowywana w odpowiednim rekordzie bazy danych. Jeśli dane są przechowywane bezpośrednio w bazie danych, przekazane dane binarne nie muszą być zapisane w systemie plików, ale muszą być dodane do bazy danych programu.

Przed rozpoczęciem kojarzenia danych z modelem danych, należy najpierw przyjrzeć się, jak przekazać dane binarne użytkownikowi końcowemu. Prezentowanie danych tekstowych jest wystarczająco proste, ale jak powinny być prezentowane dane binarne? Jest to zależne od typu danych binarnych. W przypadku obrazów najkorzystniej jest wyświetlić obraz. w przypadku plików PDF, dokumentów programu Microsoft Word, plików ZIP i innych typów danych binarnych, dzięki czemu link do pobierania jest prawdopodobnie bardziej odpowiedni.

W tym samouczku powiesz się, jak przedstawić dane binarne obok skojarzonych z nimi danych tekstowych przy użyciu kontrolek sieci Web danych, takich jak GridView i DetailsView. W następnym samouczku będziemy zachęcać do kojarzenia przekazanego pliku z bazą danych.

## <a name="step-1-providingbrochurepathvalues"></a>Krok 1. podanie`BrochurePath`wartości

Kolumna `Picture` w tabeli `Categories` zawiera już dane binarne dla różnych obrazów kategorii. W odniesieniu do kolumny `Picture` dla każdego rekordu jest przechowywana zawartość binarna obrazu o niskiej jakości i 16-kolorowej mapie bitowej. Każdy obraz kategorii ma 172 pikseli szerokości i 120 pikseli i zużywa około 11 KB. Co więcej, zawartość binarna w kolumnie `Picture` zawiera nagłówek 78-bajtowy [OLE](http://en.wikipedia.org/wiki/Object_Linking_and_Embedding) , który musi zostać usunięty przed wyświetleniem obrazu. Informacje zawarte w tym nagłówku są obecne, ponieważ baza danych Northwind ma swoje elementy główne w programie Microsoft Access. W programie Access dane binarne są przechowywane przy użyciu typu danych obiekt OLE, który jest prostopadły do tego nagłówka. Na razie zobaczymy, jak rozdzielić nagłówki z tych obrazów o niskiej jakości w celu wyświetlenia obrazu. W przyszłym samouczku utworzymy interfejs do aktualizowania kolumny kategorii `Picture` i zastąpisz te obrazy mapy bitowej, które używają nagłówków OLE z równoważnymi obrazami JPG bez zbędnych nagłówków OLE.

W poprzednim samouczku pokazano, jak używać formantu FileUpload. Z tego względu można dodać pliki broszury do systemu plików serwera sieci Web. Nie spowoduje to jednak zaktualizowania kolumny `BrochurePath` w tabeli `Categories`. W następnym samouczku zobaczymy, jak to zrobić, ale teraz musimy ręcznie podać wartości dla tej kolumny.

W tym samouczku s pobierzesz siedem plików broszury PDF w folderze `~/Brochures`, jeden dla każdej z kategorii z wyjątkiem ryby. Celowo całkowicie pominięto Dodawanie broszury dla owoców morza, aby zilustrować, jak obsługiwać scenariusze, w których nie wszystkie rekordy mają skojarzone dane binarne. Aby zaktualizować tabelę `Categories` przy użyciu tych wartości, kliknij prawym przyciskiem myszy węzeł `Categories` z Eksplorator serwera i wybierz polecenie Pokaż dane tabeli. Następnie wprowadź ścieżki wirtualne do plików broszur dla każdej kategorii, która ma broszurę, jak pokazano na rysunku 1. Ponieważ nie ma żadnej broszury dla kategorii owoce morza, pozostaw wartość `BrochurePath` Column s jako `NULL`.

[![ręcznie wprowadzić wartości dla kolumny BrochurePath tabeli kategorii](displaying-binary-data-in-the-data-web-controls-cs/_static/image1.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image1.png)

**Rysunek 1**. ręczne wprowadzanie wartości kolumny `BrochurePath` tabeli `Categories` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-binary-data-in-the-data-web-controls-cs/_static/image2.png))

## <a name="step-2-providing-a-download-link-for-the-brochures-in-a-gridview"></a>Krok 2. dostarczenie linku pobierania dla broszur w widoku GridView

Za pomocą `BrochurePath` wartości podanych dla tabeli `Categories` ponownie przygotujemy się do utworzenia widoku GridView zawierającego listę każdej kategorii wraz z linkiem umożliwiającym pobranie broszury kategorii s. W kroku 4 ten widok zostanie rozbudowany w celu wyświetlenia również obrazu kategorii s.

Zacznij od przeciągnięcia widoku GridView z przybornika do projektanta strony `DisplayOrDownloadData.aspx` w folderze `BinaryData`. Ustaw `ID` GridView s na `Categories` i za pomocą tagu inteligentnego GridView s Wybierz powiązanie go z nowym źródłem danych. W tym celu należy powiązać ją z elementem ObjectDataSource o nazwie `CategoriesDataSource`, który pobiera dane przy użyciu metody `CategoriesBLL` Object s `GetCategories()`.

[![utworzyć nowego elementu ObjectDataSource o nazwie CategoriesDataSource](displaying-binary-data-in-the-data-web-controls-cs/_static/image2.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image3.png)

**Rysunek 2**. Utwórz nowy element ObjectDataSource o nazwie `CategoriesDataSource` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-binary-data-in-the-data-web-controls-cs/_static/image4.png))

[![skonfigurować element ObjectDataSource do używania klasy CategoriesBLL](displaying-binary-data-in-the-data-web-controls-cs/_static/image3.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image5.png)

**Rysunek 3**. Konfigurowanie elementu ObjectDataSource do używania klasy `CategoriesBLL` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-binary-data-in-the-data-web-controls-cs/_static/image6.png))

[![pobrać listy kategorii za pomocą metody GetCategories ()](displaying-binary-data-in-the-data-web-controls-cs/_static/image4.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image7.png)

**Rysunek 4**. Pobieranie listy kategorii za pomocą metody `GetCategories()` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-binary-data-in-the-data-web-controls-cs/_static/image8.png))

Po zakończeniu działania Kreatora konfiguracji źródła danych program Visual Studio automatycznie doda BoundField do `Categories` GridView dla `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`i `BrochurePath` `DataColumn`. I Usuń `NumberOfProducts` BoundField, ponieważ zapytanie `GetCategories()` Metoda s nie pobiera tych informacji. Usuń również `CategoryID` BoundField i Zmień nazwę `CategoryName` i `BrochurePath` właściwości `HeaderText` odpowiednio do kategorii i broszury. Po wprowadzeniu tych zmian, znaczniki deklaratywne GridView i ObjectDataSource s powinny wyglądać następująco:

[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample1.aspx)]

Wyświetl Tę stronę za pomocą przeglądarki (patrz rysunek 5). Każda z ośmiu kategorii znajduje się na liście. Siedem kategorii z `BrochurePath` wartości mają wartość `BrochurePath` wyświetlaną w odpowiednich BoundField. Ryby, które mają `NULL` wartość dla `BrochurePath`, wyświetla pustą komórkę.

[![każdej kategorii, opis i wartość BrochurePath są wyświetlane](displaying-binary-data-in-the-data-web-controls-cs/_static/image5.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image9.png)

**Rysunek 5**. na liście są wyświetlane wszystkie kategorie, opis i wartość `BrochurePath` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-binary-data-in-the-data-web-controls-cs/_static/image10.png))

Zamiast wyświetlać tekst kolumny `BrochurePath`, chcemy utworzyć link do broszury. Aby to osiągnąć, Usuń `BrochurePath` BoundField i zastąp go HyperLinkField. Ustaw nową właściwość HyperLinkField s `HeaderText` na broszurę, jej Właściwość `Text`, aby wyświetlić broszurę i jej Właściwość `DataNavigateUrlFields` do `BrochurePath`.

![Dodaj element HyperLinkField dla BrochurePath](displaying-binary-data-in-the-data-web-controls-cs/_static/image6.gif)

**Ilustracja 6**. Dodawanie HyperLinkField do `BrochurePath`

Spowoduje to dodanie kolumny linków do widoku GridView, jak pokazano na rysunku 7. Kliknięcie linku Wyświetl broszurę spowoduje wyświetlenie pliku PDF bezpośrednio w przeglądarce lub wyświetlenie monitu o pobranie pliku w zależności od tego, czy jest zainstalowany czytnik PDF i ustawienia przeglądarki s.

[![broszurę kategorii s można wyświetlić, klikając link Wyświetl broszurę](displaying-binary-data-in-the-data-web-controls-cs/_static/image7.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image11.png)

**Rysunek 7**. Broszura kategorii s może być wyświetlana przez kliknięcie linku Wyświetl broszurę ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-binary-data-in-the-data-web-controls-cs/_static/image12.png))

[![zostanie wyświetlony plik PDF z broszurą kategorii s](displaying-binary-data-in-the-data-web-controls-cs/_static/image8.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image13.png)

**Ilustracja 8**. zostanie wyświetlony plik PDF z kategorią s ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-binary-data-in-the-data-web-controls-cs/_static/image14.png))

## <a name="hiding-the-view-brochure-text-for-categories-without-a-brochure"></a>Ukrywanie tekstu broszury dla kategorii bez broszury

Jak pokazano na rysunku 7, `BrochurePath` HyperLinkField wyświetla swoją `Text` wartość właściwości (Wyświetl broszurę) dla wszystkich rekordów, bez względu na to, czy dla `BrochurePath`nie ma`NULL` wartości. Oczywiście jeśli `BrochurePath` jest `NULL`, łącze jest wyświetlane jako tylko tekst, podobnie jak w przypadku kategorii owoce morza (patrz rysunek 7). Zamiast wyświetlania broszury widoku tekstu, warto mieć te kategorie bez `BrochurePath` wartość Wyświetla tekst alternatywny, na przykład brak dostępnej broszury.

Aby zapewnić takie zachowanie, musimy użyć elementu TemplateField, którego zawartość jest generowana za pośrednictwem wywołania metody strony, która emituje odpowiednie dane wyjściowe na podstawie wartości `BrochurePath`. Najpierw zbadamy tę technikę formatowania w samouczku [using używanie TemplateField w widoku GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) .

Przekształć HyperLinkField w TemplateField, wybierając `BrochurePath` HyperLinkField, a następnie klikając pozycję Konwertuj to pole na link TemplateField w oknie dialogowym Edytowanie kolumn.

![Konwertuj HyperLinkField na TemplateField](displaying-binary-data-in-the-data-web-controls-cs/_static/image9.gif)

**Ilustracja 9**. konwertowanie HyperLinkField na TemplateField

Spowoduje to utworzenie TemplateField z `ItemTemplate`, który zawiera kontrolkę sieci Web hiperłącze, której Właściwość `NavigateUrl` jest powiązana z wartością `BrochurePath`. Zastąp ten znacznik wywołaniem metody `GenerateBrochureLink`, przekazując wartość `BrochurePath`:

[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample2.aspx)]

Następnie Utwórz metodę `protected` w klasie ASP.NET s z kodem związanym ze stroną o nazwie `GenerateBrochureLink`, która zwraca `string` i akceptuje `object` jako parametr wejściowy.

[!code-csharp[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample3.cs)]

Ta metoda określa, czy wartość `object` zatwierdzono jest bazą danych `NULL` i, jeśli tak, zwraca komunikat informujący o braku broszury w kategorii. W przeciwnym razie, jeśli istnieje wartość `BrochurePath`, zostanie ona wyświetlona w hiperłączu. Należy pamiętać, że jeśli wartość `BrochurePath` jest obecna, jest ona przenoszona do [metody`ResolveUrl(url)`](https://msdn.microsoft.com/library/system.web.ui.control.resolveurl.aspx). Ta metoda rozwiązuje *adres URL*przekazywania, zastępując znak `~` odpowiednią ścieżką wirtualną. Na przykład jeśli aplikacja znajduje się w katalogu głównym `/Tutorial55`, `ResolveUrl("~/Brochures/Meats.pdf")` zwróci `/Tutorial55/Brochures/Meat.pdf`.

Rysunek 10 pokazuje stronę po zastosowaniu tych zmian. Zwróć uwagę, że w polu Kategoria `BrochurePath` ryby

[![tekst brak dostępnej broszury dla tych kategorii bez broszury](displaying-binary-data-in-the-data-web-controls-cs/_static/image10.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image15.png)

**Ilustracja 10**. tekst brak dostępnej broszury dla tych kategorii bez broszury ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-binary-data-in-the-data-web-controls-cs/_static/image16.png))

## <a name="step-3-adding-a-web-page-to-display-a-category-s-picture"></a>Krok 3. Dodawanie strony sieci Web w celu wyświetlenia obrazu kategorii s

Gdy użytkownik odwiedza stronę ASP.NET, otrzymuje kod HTML strony ASP.NET. Otrzymany kod HTML jest tylko tekst i nie zawiera żadnych danych binarnych. Wszelkie dodatkowe dane binarne, takie jak obrazy, pliki dźwiękowe, aplikacje Macromedia Flash, osadzone wideo z systemem Windows Media Player i tak dalej, istnieją jako oddzielne zasoby na serwerze sieci Web. KOD HTML zawiera odwołania do tych plików, ale nie obejmuje rzeczywistej zawartości plików.

Na przykład w kodzie HTML element `<img>` jest używany do odwoływania się do obrazu, z atrybutem `src` wskazującym na plik obrazu, tak więc:

[!code-html[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample4.html)]

Gdy przeglądarka otrzymuje ten kod HTML, wysyła kolejne żądanie do serwera sieci Web, aby pobrać zawartość binarną pliku obrazu, który następnie jest wyświetlany w przeglądarce. To samo pojęcie dotyczy danych binarnych. W kroku 2 broszura nie została wysłana do przeglądarki jako część znaczników HTML strony. Zamiast tego renderowane w kodzie HTML hiperłącza, które po kliknięciu spowodowały, że przeglądarka zażąda bezpośredniego dokumentu PDF.

Aby wyświetlić lub zezwolić użytkownikom na pobieranie danych binarnych, które znajdują się w bazie danych, należy utworzyć oddzielną stronę sieci Web, która zwraca dane. W przypadku naszej aplikacji wszystkie pola danych binarnych są przechowywane bezpośrednio w bazie danych. W związku z tym potrzebujemy strony, która po wywołaniu zwraca dane obrazu dla określonej kategorii.

Dodaj nową stronę ASP.NET do folderu `BinaryData` o nazwie `DisplayCategoryPicture.aspx`. W takim przypadku pozostaw pole wyboru Wybierz stronę wzorcową niezaznaczone. Ta strona oczekuje wartości `CategoryID` w ciągu QueryString i zwraca dane binarne tej kolumny `Picture` kategorii. Ponieważ ta strona zwraca dane binarne i nic nie dotyczy, nie jest potrzebne żadne znaczniki w sekcji HTML. W związku z tym kliknij kartę Źródło w lewym dolnym rogu i Usuń wszystkie znaczniki strony, z wyjątkiem dyrektywy `<%@ Page %>`. Oznacza to, że `DisplayCategoryPicture.aspx` s znaczniki deklaratywne powinny składać się z pojedynczego wiersza:

[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample5.aspx)]

Jeśli widzisz atrybut `MasterPageFile` w dyrektywie `<%@ Page %>`, usuń ją.

W klasie z kodem związanym s Dodaj następujący kod do programu obsługi zdarzeń `Page_Load`:

[!code-csharp[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample6.cs)]

Ten kod zaczyna się od odczytywania wartości `CategoryID` QueryString w zmiennej o nazwie `categoryID`. Następnie dane obrazu są pobierane za pośrednictwem wywołania metody `GetCategoryWithBinaryDataByCategoryID(categoryID)` klasy `CategoriesBLL`. Te dane są zwracane do klienta przy użyciu metody `Response.BinaryWrite(data)`, ale przed wywołaniem tej wartości należy usunąć nagłówek `Picture` kolumny wartość s. Jest to realizowane przez utworzenie tablicy `byte` o nazwie `strippedImageData`, która będzie zawierać dokładnie 78 znaków mniejszej niż wartość znajdująca się w kolumnie `Picture`. [Metoda`Array.Copy`](https://msdn.microsoft.com/library/z50k9bft.aspx) służy do kopiowania danych z `category.Picture`, zaczynając od pozycji 78 do `strippedImageData`.

Właściwość `Response.ContentType` określa [Typ MIME](http://en.wikipedia.org/wiki/MIME) zwracanej zawartości, dzięki czemu przeglądarka wie, jak ją renderować. Ponieważ kolumna `Categories` tabeli s `Picture` jest obrazem mapy bitowej, w tym miejscu jest używany typ MIME mapy bitowej (Image/BMP). Jeśli pominięto typ MIME, większość przeglądarek będzie nadal wyświetlała obraz prawidłowo, ponieważ może wywnioskować typ na podstawie zawartości danych binarnych pliku obrazu. Jednak w miarę możliwości należy uwzględnić typ MIME. Aby zapoznać się z pełną listą [typów multimediów MIME](http://www.iana.org/assignments/media-types/), zobacz [witrynę internetową Assigned Numbers Authority s](http://www.iana.org/) .

Po utworzeniu tej strony można wyświetlić informacje o określonej kategorii s, odwiedzając `DisplayCategoryPicture.aspx?CategoryID=categoryID`. Rysunek 11 przedstawia obraz kategorii napoje, który można wyświetlić w `DisplayCategoryPicture.aspx?CategoryID=1`.

[![zostanie wyświetlona ilustracja kategorii napoje s](displaying-binary-data-in-the-data-web-controls-cs/_static/image11.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image17.png)

**Ilustracja 11**. wyświetlana jest ilustracja kategorii napoje ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-binary-data-in-the-data-web-controls-cs/_static/image18.png))

Jeśli podczas odwiedzania `DisplayCategoryPicture.aspx?CategoryID=categoryID`zostanie wyświetlony wyjątek, który odczytuje nie można rzutować obiektu typu "System. DBNull" na typ "System. Byte []", istnieją dwie rzeczy, które mogą to powodować. Najpierw kolumna `Categories` tabeli `Picture` ma zezwalać na wartości `NULL`. Na stronie `DisplayCategoryPicture.aspx` jest jednak założono, że występuje nie`NULL` wartość. Nie można uzyskać dostępu bezpośrednio do właściwości `Picture` `CategoriesDataTable`, jeśli ma ona wartość `NULL`. Jeśli chcesz zezwolić na `NULL` wartości dla kolumny `Picture`, d chcesz uwzględnić następujący warunek:

[!code-csharp[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample7.cs)]

W powyższym kodzie założono, że w folderze `Images`, który ma być wyświetlany dla tych kategorii, znajduje się plik obrazu o nazwie `NoPictureAvailable.gif`.

Ten wyjątek może być również spowodowany tym, że instrukcja `CategoriesTableAdapter` s `GetCategoryWithBinaryDataByCategoryID` Metoda s `SELECT` została przywrócona do głównej listy kolumn zapytań, która może się zdarzyć, jeśli używasz instrukcji SQL ad hoc i ponownie uruchamiasz kreatora dla zapytania głównego TableAdapter s. Zaznacz, aby upewnić się, że instrukcja `GetCategoryWithBinaryDataByCategoryID` Metoda s `SELECT` nadal zawiera kolumnę `Picture`.

> [!NOTE]
> Za każdym razem, gdy `DisplayCategoryPicture.aspx` jest odwiedzana, uzyskuje się dostęp do bazy danych i zwracane są określone dane obrazu. Jeśli obraz kategorii s nie został zmieniony od czasu ostatniego wyświetlenia przez użytkownika, to jest to tracony nakład pracy. Na szczęście protokół HTTP zezwala na *pobieranie warunkowe*. W przypadku warunkowego GET klient wysyła żądanie HTTP do [`If-Modified-Since` nagłówku HTTP](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html) , który zapewnia datę i godzinę ostatniego pobrania przez klienta tego zasobu z serwera sieci Web. Jeśli zawartość nie zmieniła się od określonej daty, serwer sieci Web może odpowiedzieć [niezmodyfikowanym kodem stanu (304)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) i forgo, wysyłając ponownie żądaną zawartość zasobu. W krótkim przypadku ta technika zwalnia serwer sieci Web od konieczności wysyłania zawartości z powrotem do zasobu, jeśli nie został on zmodyfikowany od momentu ostatniego dostępu do niego przez klienta.

Jednak w celu zaimplementowania tego zachowania wymagane jest dodanie kolumny `PictureLastModified` do tabeli `Categories` w celu przechwycenia, gdy kolumna `Picture` została ostatnio zaktualizowana, a także kod do sprawdzenia nagłówka `If-Modified-Since`. Aby uzyskać więcej informacji na temat nagłówka `If-Modified-Since` i przepływu pracy warunkowego GET, zobacz [http Conditional Get for RSS hakerzy](http://fishbowl.pastiche.org/2002/10/21/http_conditional_get_for_rss_hackers) i [bardziej szczegółowy sposób wykonywania żądań HTTP na stronie ASP.NET](http://aspnet.4guysfromrolla.com/articles/122204-1.aspx).

## <a name="step-4-displaying-the-category-pictures-in-a-gridview"></a>Krok 4. Wyświetlanie obrazów kategorii w widoku GridView

Teraz, gdy mamy stronę sieci Web do wyświetlania określonego obrazu kategorii s, możemy ją wyświetlić przy użyciu [kontrolki sieci Web obrazu](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/image.aspx) lub elementu HTML `<img>` wskazującego `DisplayCategoryPicture.aspx?CategoryID=categoryID`. Obrazy, których adres URL jest określany przez dane bazy danych, mogą być wyświetlane w widoku GridView lub DetailsView przy użyciu ImageField. ImageField zawiera `DataImageUrlField` i `DataImageUrlFormatString` właściwości, które działają podobnie jak `DataNavigateUrlFields` HyperLinkField s i `DataNavigateUrlFormatString` właściwości.

Pozwól s rozszerzyć `Categories` GridView w `DisplayOrDownloadData.aspx` przez dodanie elementu ImageField w celu wyświetlenia każdego obrazu kategorii s. Po prostu Dodaj ImageField i ustaw jego właściwości `DataImageUrlField` i `DataImageUrlFormatString` na odpowiednio `CategoryID` i `DisplayCategoryPicture.aspx?CategoryID={0}`. Spowoduje to utworzenie kolumny GridView, która renderuje `<img>` element, którego atrybut `src` odwołuje się `DisplayCategoryPicture.aspx?CategoryID={0}`, gdzie {0} zostanie zamieniony na wartość `CategoryID` wiersza GridView.

![Dodaj element ImageField do widoku GridView](displaying-binary-data-in-the-data-web-controls-cs/_static/image12.gif)

**Ilustracja 12**. Dodawanie ImageField do widoku GridView

Po dodaniu ImageField, składnia deklaratywna GridView powinna wyglądać podobnie do Soothe:

[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample8.aspx)]

Poświęć chwilę na wyświetlenie tej strony za pomocą przeglądarki. Zwróć uwagę na to, jak każdy rekord zawiera teraz obraz dla kategorii.

[![zostanie wyświetlona kategoria s obrazu dla każdego wiersza](displaying-binary-data-in-the-data-web-controls-cs/_static/image13.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image19.png)

**Ilustracja 13**. obraz kategorii s jest wyświetlany dla każdego wiersza ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-binary-data-in-the-data-web-controls-cs/_static/image20.png))

## <a name="summary"></a>Podsumowanie

W tym samouczku zbadano sposób prezentowania danych binarnych. Sposób przedstawiania danych zależy od typu danych. Dla plików broszury PDF oferujemy link do wyświetlania broszury, który po kliknięciu powoduje, że Użytkownik pobrał bezpośrednio do pliku PDF. W przypadku obrazu kategorii s najpierw została utworzona strona umożliwiająca pobranie i zwrócenie danych binarnych z bazy danych, a następnie użycie tej strony do wyświetlania każdego obrazu kategorii s w widoku GridView.

Teraz, gdy przeglądamy sposób wyświetlania danych binarnych, zagotowuje się do sprawdzenia, jak przeprowadzić Wstawianie, aktualizowanie i usuwanie bazy danych z danymi binarnymi. W następnym samouczku zawarto informacje na temat kojarzenia przekazanego pliku z odpowiadającym mu rekordem bazy danych. W tym samouczku zobaczymy, jak zaktualizować istniejące dane binarne, a także jak usunąć dane binarne po usunięciu skojarzonego rekordu.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci liderzy dla tego samouczka to Teresa Murphy i Dave Gardner. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](uploading-files-cs.md)
> [dalej](including-a-file-upload-option-when-adding-a-new-record-cs.md)

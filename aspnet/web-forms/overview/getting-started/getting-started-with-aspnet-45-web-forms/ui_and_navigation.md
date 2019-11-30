---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
title: Interfejs użytkownika i nawigacja | Microsoft Docs
author: Erikre
description: Ta seria samouczków zawiera informacje na temat tworzenia aplikacji ASP.NET Web Forms przy użyciu ASP.NET 4,5 i Microsoft Visual Studio Express 2013...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 5c76891d-e515-4885-b576-76bd2c494efe
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
msc.type: authoredcontent
ms.openlocfilehash: ac1dcaf1ba911fdcaeb3845c6836ec771733d93e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74636820"
---
# <a name="ui-and-navigation"></a>Interfejs użytkownika i nawigacja

Autor [Erik Reitan](https://github.com/Erikre)

[Pobierz program Wingtip zabawki (C#)](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) lub [Pobierz książkę elektroniczną (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Ta seria samouczków zawiera informacje na temat tworzenia aplikacji ASP.NET Web Forms przy użyciu ASP.NET 4,5 i Microsoft Visual Studio Express 2013 dla sieci Web. Projekt Visual Studio 2013 [z C# kodem źródłowym](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) jest dostępny do tej serii samouczków.

W tym samouczku zmodyfikujesz interfejs użytkownika domyślnej aplikacji sieci Web do obsługi funkcji aplikacji Wingtip w sklepie. Ponadto zostanie dodana prosta i powiązana z danymi nawigacja. Ten samouczek kompiluje się w poprzednim samouczku "Tworzenie warstwy dostępu do danych" i jest częścią serii samouczków Wingtip.

## <a name="what-youll-learn"></a>Dowiesz się:

- Jak zmienić interfejs użytkownika, aby obsługiwał funkcje aplikacji Wingtip w sklepie.
- Jak skonfigurować element HTML5 w taki sposób, aby obejmował nawigację po stronie.
- Jak utworzyć kontrolkę opartą na danych w celu przechodzenia do określonych danych produktu.
- Jak wyświetlać dane z bazy danych utworzonej przy użyciu Entity Framework Code First.

Formularze sieci Web ASP.NET umożliwiają tworzenie zawartości dynamicznej dla aplikacji sieci Web. Każda Strona sieci Web ASP.NET jest tworzona w sposób podobny do statycznej strony sieci Web HTML (strona, która nie zawiera przetwarzania na serwerze), ale strona sieci Web ASP.NET zawiera dodatkowe elementy, które ASP.NET rozpoznaje i przetwarza kod HTML podczas uruchamiania strony.

Ze statyczną stroną HTML (plik *. htm* lub *. html* ) serwer spełnia żądanie `Web`, odczytując plik i wysyłając go jako-is do przeglądarki. Natomiast gdy ktoś zażąda strony sieci Web ASP.NET (plik *. aspx* ), strona jest uruchamiana jako program na serwerze sieci Web. Gdy strona jest uruchomiona, może wykonać dowolne zadanie wymagane przez witrynę sieci Web, w tym obliczać wartości, czytać lub zapisywać informacje o bazie danych lub wywołując inne programy. Jako dane wyjściowe strona dynamicznie tworzy znaczniki (takie jak elementy w języku HTML) i wysyła je do przeglądarki.

## <a name="modifying-the-ui"></a>Modyfikowanie interfejsu użytkownika

Tę serię samouczków będziesz kontynuować, modyfikując *domyślną stronę. aspx* . Zmodyfikujesz interfejs użytkownika, który został już ustanowiony przez szablon domyślny użyty do utworzenia aplikacji. Typ modyfikacji, które będą tworzone są typowe podczas tworzenia dowolnej aplikacji formularzy sieci Web. Można to zrobić, zmieniając tytuł, zastępując część zawartości i usuwając niepotrzebną zawartość domyślną.

1. Otwórz lub przejdź do domyślnej strony. *aspx* .
2. Jeśli strona jest wyświetlana w widoku **projektu** , przejdź do widoku **Źródło** .
3. W górnej części strony w `@Page` dyrektywie Zmień atrybut `Title` na "Welcome", jak pokazano poniżej. 

    [!code-aspx[Main](ui_and_navigation/samples/sample1.aspx?highlight=1)]
4. Na stronie *default. aspx* Zastąp całą domyślną zawartość zawartą w tagu `<asp:Content>`, aby znaczniki pojawiły się poniżej. 

    [!code-aspx[Main](ui_and_navigation/samples/sample2.aspx)]
5. Zapisz *domyślną stronę. aspx* , wybierając pozycję **Zapisz Default. aspx** z menu **plik** .

   Utworzona na poniższej stronie *domyślna. aspx* zostanie wyświetlona w następujący sposób: 

[!code-aspx[Main](ui_and_navigation/samples/sample3.aspx)]

W przykładzie ustawiono atrybut `Title` dyrektywy `@Page`. Po wyświetleniu w przeglądarce kodu HTML kod serwera `<%: Page.Title %>` jest rozpoznawany jako zawartość znajdująca się w atrybucie `Title`.

Przykładowa strona zawiera podstawowe elementy, które stanowią stronę sieci Web ASP.NET. Strona zawiera tekst statyczny, ponieważ może znajdować się na stronie HTML wraz z elementami, które są specyficzne dla ASP.NET. Zawartość znajdująca się na stronie *default. aspx* zostanie zintegrowana z zawartością strony głównej, która zostanie omówiona w dalszej części tego samouczka.

### <a name="page-directive"></a>@Page — dyrektywa

ASP.NET Web Forms zwykle zawiera dyrektywy, które umożliwiają określenie właściwości strony i informacji o konfiguracji dla strony. Dyrektywy są używane przez ASP.NET jako instrukcje dotyczące sposobu przetwarzania strony, ale nie są renderowane jako część znaczników wysyłanych do przeglądarki.

Najczęściej używaną dyrektywą jest `@Page` dyrektywie, która umożliwia określenie wielu opcji konfiguracji dla strony, w tym następujących:

1. Język programowania serwera dla kodu na stronie, na przykład C#.
2. Czy strona jest stroną z kodem serwera bezpośrednio na stronie, która nazywa się jednoplikową stroną lub czy jest stroną z kodem w osobnym pliku klasy, który jest nazywany stroną powiązanym z kodem.
3. Określa, czy strona ma skojarzoną stronę wzorcową i dlatego powinna być traktowana jako strona zawartości.
4. Opcje debugowania i śledzenia.

Jeśli nie dołączysz dyrektywy `@Page` na stronie lub dyrektywa nie zawiera określonego ustawienia, ustawienie zostanie Odziedziczone z pliku konfiguracyjnego *Web. config* lub pliku konfiguracji *Machine. config* . Plik *Machine. config* udostępnia dodatkowe ustawienia konfiguracji dla wszystkich aplikacji uruchomionych na komputerze.

> [!NOTE] 
> 
> *Plik Machine. config* zawiera również szczegółowe informacje o wszystkich możliwych ustawieniach konfiguracji.

### <a name="web-server-controls"></a>Kontrolki serwera sieci Web

W większości ASP.NET aplikacji formularzy sieci Web dodasz kontrolki, które umożliwiają użytkownikowi współpracującie ze stroną, taką jak przyciski, pola tekstowe, listy itd. Te kontrolki serwera sieci Web są podobne do przycisków HTML i elementów wejściowych. Są one jednak przetwarzane na serwerze, co pozwala na używanie kodu serwera do ustawiania ich właściwości. Te kontrolki powodują również wywoływanie zdarzeń, które można obsłużyć w kodzie serwera.

Formanty serwera używają specjalnej składni, która jest rozpoznawana przez ASP.NET podczas uruchamiania strony. Nazwa tagu dla kontrolek serwera ASP.NET rozpoczyna się od prefiksu `asp:`. Dzięki temu ASP.NET może rozpoznać i przetworzyć te kontrolki serwerowe. Prefiks może być inny, jeśli formant nie jest częścią .NET Framework. Oprócz prefiksu `asp:`, formanty serwera ASP.NET obejmują również atrybut `runat="server"` i `ID`, za pomocą którego można odwołać się do formantu w kodzie serwera.

Gdy strona zostanie uruchomiona, ASP.NET identyfikuje kontrolki serwera i uruchamia kod, który jest skojarzony z tymi kontrolkami. Wiele kontrolek renderuje niektóre HTML lub inne znaczniki na stronie, gdy zostanie ona wyświetlona w przeglądarce.

### <a name="server-code"></a>Kod serwera

Większość aplikacji ASP.NET Web Forms obejmuje kod, który jest uruchamiany na serwerze podczas przetwarzania strony. Jak wspomniano powyżej, kod serwera może służyć do wykonywania różnych zadań, takich jak dodawanie danych do kontrolki ListView. Program ASP.NET obsługuje wiele języków, które są uruchamiane na serwerze C#, w tym Visual Basic, J# i innych.

ASP.NET obsługuje dwa modele do pisania kodu serwera dla strony sieci Web. W modelu pojedynczego pliku, kod strony znajduje się w elemencie skryptu, w którym tag otwierający zawiera atrybut `runat="server"`. Alternatywnie można utworzyć kod strony w osobnym pliku klasy, który jest określany jako model związany z kodem. W takim przypadku strona formularzy sieci Web ASP.NET na ogół nie zawiera kodu serwera. Zamiast tego dyrektywa `@Page` zawiera informacje, które łączą stronę *aspx* ze skojarzonym z nią plikiem związanym z kodem.

Atrybut `CodeBehind` zawarty w dyrektywie `@Page` określa nazwę oddzielnego pliku klasy, a atrybut `Inherits` określa nazwę klasy w pliku związanym z kodem, który odpowiada stronie.

### <a name="updating-the-master-page"></a>Aktualizowanie strony wzorcowej

W programie ASP.NET Web Forms strony wzorcowe umożliwiają tworzenie spójnego układu dla stron w aplikacji. Pojedyncza strona wzorcowa definiuje wygląd i działanie oraz standardowe zachowanie dla wszystkich stron (lub grupy stron) w aplikacji. Następnie można utworzyć poszczególne strony zawartości zawierające zawartość, która ma zostać wyświetlona, zgodnie z powyższym opisem. Gdy użytkownicy zażądają stron zawartości, ASP.NET Scala je ze stroną wzorcową w celu utworzenia danych wyjściowych, które łączą układ strony wzorcowej z zawartością ze strony zawartość.

Nowa witryna wymaga jednego logo do wyświetlania na każdej stronie. Aby dodać to logo, możesz zmodyfikować kod HTML na stronie wzorcowej.

1. W **Eksplorator rozwiązań**Znajdź i Otwórz stronę **site. Master** .
2. Jeśli strona jest w widoku **projektu** , przejdź do widoku **Źródło** .
3. Zaktualizuj stronę wzorcową, **modyfikując lub dodając** znaczniki wyróżnione kolorem żółtym: 

    [!code-aspx[Main](ui_and_navigation/samples/sample4.aspx?highlight=9,49,76-81,87)]

W tym kodzie HTML zostanie wyświetlony obraz o nazwie *logo. jpg* z folderu *obrazy* aplikacji sieci Web, który zostanie później dodany. Gdy w przeglądarce zostanie wyświetlona strona, która używa strony głównej, zostanie wyświetlone logo. Jeśli użytkownik kliknie logo, użytkownik przejdzie z powrotem do domyślnej strony. *aspx* . Tag kotwicy HTML `<a>` otacza formant serwera obrazu i umożliwia uwzględnienie obrazu jako części linku. Atrybut `href` dla tagu kotwicy określa główny "`~/`" witryny sieci Web jako lokalizację linku. Domyślnie zostanie wyświetlona strona *default. aspx* , gdy użytkownik przechodzi do katalogu głównego witryny sieci Web. Kontrolka serwer `<asp:Image>` **obrazu** obejmuje Dodawanie właściwości, takich jak `BorderStyle`, które są renderowane jako kod HTML, gdy jest wyświetlany w przeglądarce.

### <a name="master-pages"></a>Strony wzorcowe

Strona wzorcowa to plik ASP.NET z rozszerzeniem Master (na przykład *site. Master*) ze wstępnie zdefiniowanym układem, który może zawierać tekst statyczny, elementy HTML i kontrolki serwera. Strona wzorcowa jest identyfikowana za pomocą specjalnej dyrektywy `@Master`, która zastępuje dyrektywę `@Page`, która jest używana dla zwykłych stron *. aspx* .

Oprócz dyrektywy `@Master` Strona wzorcowa zawiera również wszystkie elementy HTML najwyższego poziomu dla strony, takie jak `html`, `head`i `form`. Na przykład na stronie wzorcowej, która została dodana powyżej, używasz `table` HTML do układu, elementu `img` dla logo firmy, tekstu statycznego i kontrolek serwera do obsługi wspólnych członkostw dla witryny. Możesz użyć dowolnego kodu HTML i dowolnego elementu ASP.NET jako części strony wzorcowej.

Oprócz tekstu statycznego i kontrolek, które będą wyświetlane na wszystkich stronach, Strona wzorcowa zawiera również co najmniej jedną kontrolkę **ContentPlaceHolder** . Te kontrolki symbolu zastępczego definiują regiony, w których zostanie wyświetlona zawartość. Z kolei zawartość do przemieszczenia jest definiowana na stronach zawartości, takich jak *default. aspx*, przy użyciu kontrolki serwer **zawartości** .

#### <a name="adding-image-files"></a>Dodawanie plików obrazów

Obraz logo, do którego odwołuje się powyżej, wraz ze wszystkimi obrazami produktu, musi zostać dodany do aplikacji sieci Web, aby mogły być widoczne, gdy projekt jest wyświetlany w przeglądarce.

#### <a name="download-from-msdn-samples-site"></a>Pobierz ze strony przykładów MSDN:

[Wprowadzenie z formularzami sieci Web ASP.NET 4,5 i zabawkami Visual Studio 2013-Wingtip](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)

Pobieranie obejmuje zasoby w folderze *WingtipToys-Assets* , które są używane do tworzenia przykładowej aplikacji.

1. Jeśli jeszcze tego nie zrobiono, Pobierz skompresowane pliki przykładowe przy użyciu powyższego linku w witrynie przykładów MSDN.
2. Po pobraniu pliku. zip i skopiuj zawartość do folderu lokalnego na komputerze.
3. Znajdź i Otwórz folder *WingtipToys-Assets* .
4. Przeciągając i upuszczając, skopiuj folder *wykazu* z folderu lokalnego do katalogu głównego projektu aplikacji sieci Web w **Eksplorator rozwiązań** programu Visual Studio. 

    ![Interfejs użytkownika i nawigacja — Kopiuj pliki](ui_and_navigation/_static/image1.png)
5. Następnie utwórz nowy folder o nazwie *images* , klikając prawym przyciskiem myszy projekt **WingtipToys** w **Eksplorator rozwiązań** i wybierając pozycję **Dodaj** -&gt; **nowym folderze**.
6. Skopiuj plik *logo. jpg* z folderu *WingtipToys-Assets* w **Eksploratorze plików** do folderu *images* w projekcie aplikacji sieci Web w **Eksplorator rozwiązań** programu Visual Studio.
7. Kliknij opcję **Pokaż wszystkie pliki** w górnej części **Eksplorator rozwiązań** , aby zaktualizować listę plików, jeśli nie widzisz nowych plików.  
  
    **Eksplorator rozwiązań** teraz wyświetla zaktualizowane pliki projektu. 

    ![Interfejs użytkownika i nawigacja — Eksplorator rozwiązań](ui_and_navigation/_static/image2.png)

### <a name="adding-pages"></a>Dodawanie stron

Przed dodaniem nawigacji do aplikacji sieci Web należy najpierw dodać dwie nowe strony, do których będziesz przechodzić. W dalszej części tej serii samouczków zostaną wyświetlone produkty i szczegóły dotyczące produktów na tych nowych stronach.

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy pozycję **WingtipToys**, kliknij pozycję **Dodaj**, a następnie kliknij pozycję **nowy element**.   
 Zostanie wyświetlone okno dialogowe **Dodaj nowy element** .
2. Wybierz grupę szablonów **sieci Web** **Visual C#**  -&gt; po lewej stronie. Następnie wybierz pozycję **formularz sieci Web ze stroną wzorcową** z listy Środkowej i nadaj jej nazwę *ProductList. aspx*. 

    ![Interfejs użytkownika i nawigacja — okno dialogowe Dodawanie nowego elementu](ui_and_navigation/_static/image3.png)
3. Wybierz pozycję **site. Master** , aby dołączyć stronę wzorcową do nowo utworzonej strony *. aspx* . 

    ![Interfejs użytkownika i nawigacja — wybierz stronę wzorcową](ui_and_navigation/_static/image4.png)
4. Dodaj dodatkową stronę o nazwie *ProductDetails. aspx* , wykonując następujące kroki.

### <a name="updating-bootstrap"></a>Aktualizowanie ładowania początkowego

Szablony projektów Visual Studio 2013 używają [Bootstrap](http://getbootstrap.com/), układu i struktury z motywem utworzonym przez serwis Twitter. Funkcja ładowania początkowego korzysta z CSS3 w celu zapewnienia, że program umożliwia dynamiczne dostosowanie do różnych rozmiarów okna przeglądarki. Możesz również użyć funkcji obsługi motywów Bootstrap, aby łatwo zmienić wygląd i działanie aplikacji. Domyślnie szablon aplikacji sieci Web ASP.NET w Visual Studio 2013 zawiera polecenie Bootstrap jako pakiet NuGet.

W tym samouczku zmienisz wygląd i działanie aplikacji Wingtip zabawki, zastępując pliki w pliku CSS.

1. W **Eksplorator rozwiązań**Otwórz folder *zawartości* .
2. Kliknij prawym przyciskiem myszy plik *Bootstrap. css* i zmień jego nazwę na *Bootstrap-Original. css*.
3. Zmień nazwę pliku *Bootstrap. min. css* na *Bootstrap-Original. min. css*.
4. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder *Content (zawartość* ) i wybierz polecenie **Otwórz folder w Eksploratorze plików**.  
   Zostanie wyświetlony Eksplorator plików. Pobrano pliki CSS Bootstrap do tej lokalizacji.
5. W przeglądarce przejdź do [https://bootswatch.com/3/](https://bootswatch.com/3/).
6. Przewiń okno przeglądarki do momentu wyświetlenia motywu Cerulean. 

    ![Interfejs użytkownika i nawigacja — motyw Cerulean](ui_and_navigation/_static/image5.png)
7. Pobierz plik *Bootstrap. css* i plik *Bootstrap. min. css* do folderu *Content* . Użyj ścieżki do folderu zawartości, który jest wyświetlany w otwartym wcześniej oknie **Eksploratora plików** .
8. W programie **Visual Studio** w górnej części **Eksplorator rozwiązań**wybierz opcję **Pokaż wszystkie pliki** , aby wyświetlić nowe pliki w folderze zawartości. 

    ![Interfejs użytkownika i nawigacja — Eksplorator rozwiązań](ui_and_navigation/_static/image6.png)

   Zostaną wyświetlone dwa nowe pliki CSS w folderze **zawartości** , ale zauważ, że ikona obok każdej nazwy pliku jest wyszarzona. Oznacza to, że plik nie został jeszcze dodany do projektu.
9. Kliknij prawym przyciskiem myszy plik *Bootstrap. css* i pliki *Bootstrap. min. css* , a następnie wybierz opcję **Dołącz do projektu**.   
   Po uruchomieniu aplikacji Wingtip zabawki w dalszej części tego samouczka zostanie wyświetlony nowy interfejs użytkownika.

> [!NOTE] 
> 
> Szablon aplikacji sieci Web ASP.NET używa pliku *pakietu. config* w katalogu głównym projektu w celu przechowywania ścieżki plików CSS Bootstrap.

### <a name="modifying-the-default-navigation"></a>Modyfikowanie nawigacji domyślnej

Domyślną nawigację dla każdej strony w aplikacji można zmodyfikować, zmieniając nieuporządkowany element listy nawigacji, który znajduje się na stronie *site. Master* .

1. W **Eksplorator rozwiązań**zlokalizuj i Otwórz stronę *site. Master* .
2. Dodanie dodatkowego linku nawigacyjnego wyróżnionego na żółto do listy nieuporządkowanej wyświetlonej poniżej:   

    [!code-html[Main](ui_and_navigation/samples/sample5.html?highlight=5)]

Jak widać w powyższym kodzie HTML, każdy element wiersza `<li>` zawierający tag kotwicy `<a>` z atrybutem `href` linku. Każdy `href` wskazuje stronę w aplikacji sieci Web. W przeglądarce, gdy użytkownik kliknie jedno z tych linków (na przykład **produkty**), przejdzie do strony zawartej w `href` (na przykład **ProductList. aspx**). Aplikacja zostanie uruchomiona na końcu tego samouczka.

> [!NOTE] 
> 
> Znak tyldy (`~`) służy do określenia, że ścieżka `href` rozpoczyna się w katalogu głównym projektu.

### <a name="adding-a-data-control-to-display-navigation-data"></a>Dodawanie kontrolki danych do wyświetlania danych nawigacyjnych

Następnie dodasz kontrolkę, aby wyświetlić wszystkie kategorie z bazy danych. Każda kategoria będzie pełnić rolę linku do strony *ProductList. aspx* . Gdy użytkownik kliknie link kategorii w przeglądarce, przejdzie do strony produkty i zobaczy tylko produkty skojarzone z wybraną kategorią.

Użyjesz kontrolki **ListView** , aby wyświetlić wszystkie kategorie zawarte w bazie danych. Aby dodać formant **ListView** do strony głównej:

1. Na stronie *site. Master* Dodaj następujący wyróżniony element `<div>` **po** elemencie `<div>` zawierającym `id="TitleContent"` dodany wcześniej:  

    [!code-aspx[Main](ui_and_navigation/samples/sample6.aspx?highlight=7-21)]

Ten kod będzie wyświetlał wszystkie kategorie z bazy danych. Kontrolka **ListView** wyświetla nazwę kategorii jako tekst łącza i zawiera łącze do strony *ProductList. aspx* z wartością ciągu zapytania zawierającego `ID` kategorii. Ustawienie właściwości `ItemType` w formancie **ListView** powoduje, że wyrażenie powiązania danych `Item` jest dostępne w węźle `ItemTemplate` i formant zostanie jednoznacznie określony. Możesz wybrać szczegóły obiektu `Item` przy użyciu funkcji IntelliSense, na przykład określając `CategoryName`. Ten kod jest zawarty wewnątrz kontenera `<%#: %>`, który oznacza wyrażenie powiązania danych. Przez dodanie (:) na końcu prefiksu `<%#`, wynikiem wyrażenia powiązania danych jest kodowanie HTML. Gdy wynikiem jest kodowanie HTML, aplikacja jest lepiej chroniona przed atakami polegającymi na iniekcji skryptów między lokacjami i iniekcją kodu HTML.

> [!NOTE] 
> 
> **Wyowietlon**
> 
> Po dodaniu kodu przez wpisanie podczas opracowywania, można mieć pewność, że zostanie znaleziony prawidłowy element członkowski obiektu, ponieważ kontrolki danych z jednoznacznie określonymi typami pokazują dostępne elementy członkowskie w oparciu o technologię IntelliSense. Funkcja IntelliSense oferuje odpowiednie dla kontekstu opcje kodu podczas wpisywania kodu, takiego jak właściwości, metody i obiekty.

W następnym kroku zostanie zaimplementowana Metoda `GetCategories` w celu pobrania danych.

### <a name="linking-the-data-control-to-the-database"></a>Łączenie kontroli danych z bazą danych

Aby można było wyświetlić dane w kontrolce danych, należy połączyć formant danych z bazą danych. Aby utworzyć łącze, można zmodyfikować kod znajdujący się w pliku *site.Master.cs* .

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy stronę *site. Master* , a następnie kliknij polecenie **Wyświetl kod**. Plik *site.Master.cs* zostanie otwarty w edytorze.
2. Na początku pliku *site.Master.cs* Dodaj dwa dodatkowe przestrzenie nazw, aby wszystkie uwzględnione przestrzenie nazw pojawiły się w następujący sposób:  

    [!code-csharp[Main](ui_and_navigation/samples/sample7.cs?highlight=8-9)]
3. Dodaj wyróżnioną metodę `GetCategories` po obsłudze zdarzeń `Page_Load` w następujący sposób:  

    [!code-csharp[Main](ui_and_navigation/samples/sample8.cs?highlight=6-11)]

Powyższy kod jest wykonywany, gdy każda Strona, która używa strony wzorcowej, jest załadowana w przeglądarce. Kontrolka `ListView` (o nazwie "categoryList"), która została dodana wcześniej w tym samouczku, używa powiązania modelu, aby wybrać dane. W znaczniku kontrolki `ListView` Właściwość `SelectMethod` kontrolki jest ustawiana na metodę `GetCategories`, jak pokazano powyżej. Formant `ListView` wywołuje metodę `GetCategories` w odpowiednim czasie w cyklu życia strony i automatycznie wiąże zwrócone dane. Dowiesz się więcej na temat powiązań danych w następnym samouczku.

### <a name="running-the-application-and-creating-the-database"></a>Uruchamianie aplikacji i tworzenie bazy danych

Wcześniej w tej serii samouczków została utworzona Klasa inicjatora (o nazwie "ProductDatabaseInitializer"), która została określona w pliku *Global.asax.cs* . Entity Framework będzie generować bazę danych, gdy aplikacja jest uruchamiana po raz pierwszy, ponieważ metoda `Application_Start` zawarta w pliku *Global.asax.cs* wywoła klasę inicjatora. Klasa inicjatora będzie używać klas modeli (`Category` i `Product`), które zostały dodane wcześniej w tej serii samouczków, aby utworzyć bazę danych.

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy stronę *default. aspx* , a następnie wybierz pozycję **Ustaw jako stronę startową**.
2. W programie Visual Studio naciśnij klawisz **F5**.   
 Ustawienie wszystkiego potrwa trochę czasu podczas pierwszego uruchomienia.   
    ![interfejs użytkownika i nawigacja — Windows](ui_and_navigation/_static/image7.png)  
 Po uruchomieniu aplikacji aplikacja zostanie skompilowana, a baza danych o nazwie *wingtiptoys. mdf* zostanie utworzona w folderze *danych\_aplikacji* . W przeglądarce zobaczysz menu nawigacji kategorii. To menu zostało wygenerowane przez pobranie kategorii z bazy danych. W następnym samouczku zostanie zaimplementowana nawigacja.
3. Zamknij przeglądarkę, aby zatrzymać uruchomioną aplikację.

### <a name="reviewing-the-database"></a>Przeglądanie bazy danych

Otwórz plik *Web. config* i zapoznaj się z sekcją parametrów połączenia. Można zobaczyć, że wartość `AttachDbFilename` w parametrach połączenia wskazuje `DataDirectory` dla projektu aplikacji sieci Web. Wartość `|DataDirectory|` jest wartością zastrzeżoną, która reprezentuje folder *danych\_aplikacji* w projekcie. W tym folderze znajduje się baza danych, która została utworzona na podstawie klas jednostek.

[!code-xml[Main](ui_and_navigation/samples/sample9.xml)]

> [!NOTE] 
> 
> Jeśli folder *danych\_aplikacji* nie jest widoczny lub jeśli folder jest pusty, wybierz ikonę **odświeżania** , a następnie ikonę **Pokaż wszystkie pliki** w górnej części okna **Eksplorator rozwiązań** . Aby wyświetlić wszystkie dostępne ikony, można powiększać szerokość okien **Eksplorator rozwiązań** .

Teraz można sprawdzić dane zawarte w pliku bazy danych *wingtiptoys. mdf* przy użyciu okna **Eksplorator serwera** .

1. Rozwiń folder *dane\_aplikacji* . Jeśli folder *danych\_aplikacji* nie jest widoczny, zapoznaj się z uwagą powyżej.
2. Jeśli plik bazy danych *wingtiptoys. mdf* nie jest widoczny, wybierz ikonę **odświeżania** , a następnie ikonę **Pokaż wszystkie pliki** w górnej części okna **Eksplorator rozwiązań** .
3. Kliknij prawym przyciskiem myszy plik bazy danych *wingtiptoys. mdf* i wybierz polecenie **Otwórz**.  
    Zostanie wyświetlona **Eksplorator serwera** . 

    ![Interfejs użytkownika i nawigacja — Eksplorator serwera](ui_and_navigation/_static/image8.png)
4. Rozwiń folder *tabele* .
5. Kliknij prawym przyciskiem myszy tabelę **Products (produkty**) i wybierz polecenie **Pokaż dane tabeli**.  
 Zostanie wyświetlona tabela **produkty** . 

    ![Interfejs użytkownika i nawigacja — tabela produktów](ui_and_navigation/_static/image9.png)
6. Ten widok umożliwia ręczne wyświetlenie i zmodyfikowanie danych w tabeli **Products** .
7. Zamknij okno tabeli **produkty** .
8. W **Eksplorator serwera**kliknij prawym przyciskiem myszy tabelę **Products (produkty** ), a następnie wybierz polecenie **Otwórz definicję tabeli**.  
 Zostanie wyświetlony projekt danych w tabeli **Products** . 

    ![Interfejs użytkownika i nawigacja — projekt produktów](ui_and_navigation/_static/image10.png)
9. Na karcie **T-SQL** zostanie wyświetlona instrukcja SQL DDL użyta do utworzenia tabeli. Możesz również użyć interfejsu użytkownika na karcie **projektowanie** , aby zmodyfikować schemat.
10. W **Eksplorator serwera**kliknij prawym przyciskiem myszy pozycję baza danych **WingtipToys** i wybierz pozycję **Zamknij połączenie**.   
 Odłączenie bazy danych od programu Visual Studio spowoduje, że schemat bazy danych będzie można zmodyfikować w dalszej części tej serii samouczków.
11. Wróć do **Eksplorator rozwiązań**, wybierając kartę **Eksplorator rozwiązań** w dolnej części okna **Eksplorator serwera** .

## <a name="summary"></a>Podsumowanie

W tym samouczku dotyczącym serii dodano kilka podstawowych interfejsów użytkownika, grafiki, stron i nawigacji. Ponadto uruchomiono aplikację sieci Web, która utworzyła bazę danych z klas danych, które zostały dodane w poprzednim samouczku. Zawartość tabeli *Products* bazy danych jest również przeglądana bezpośrednio przez bezpośrednie wyświetlanie bazy danych. W następnym samouczku zostaną wyświetlone elementy danych i szczegóły z bazy danych.

## <a name="additional-resources"></a>Dodatkowe materiały

[Wprowadzenie do programowania ASP.NET stron sieci Web](https://msdn.microsoft.com/library/ms178125.aspx)   
[Przegląd kontrolek serwera sieci Web ASP.NET](https://msdn.microsoft.com/library/zsyt68f1.aspx)   
[Samouczek CSS](http://www.w3schools.com/css/default.asp)

> [!div class="step-by-step"]
> [Poprzednie](create_the_data_access_layer.md)
> [dalej](display_data_items_and_details.md)

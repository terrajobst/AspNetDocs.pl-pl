---
uid: web-forms/overview/data-access/introduction/master-pages-and-site-navigation-cs
title: Strony wzorcowe i nawigacja wC#witrynie () | Microsoft Docs
author: rick-anderson
description: Jedną z typowych cech witryn sieci Web przyjaznych dla użytkownika jest to, że mają one spójny układ strony i schemat nawigacji dla całej lokacji. Ten samouczek wygląda na to, jak y...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 5aee8202-a4e3-4aa9-8a95-cd5d156cea4c
msc.legacyurl: /web-forms/overview/data-access/introduction/master-pages-and-site-navigation-cs
msc.type: authoredcontent
ms.openlocfilehash: e1ddd43524a61ff2e012171eba1a8dc8efbf8f1d
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74587531"
---
# <a name="master-pages-and-site-navigation-c"></a>Strony wzorcowe i nawigacja po witrynie (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_3_CS.exe) lub [Pobierz plik PDF](master-pages-and-site-navigation-cs/_static/datatutorial03cs1.pdf)

> Jedną z typowych cech witryn sieci Web przyjaznych dla użytkownika jest to, że mają one spójny układ strony i schemat nawigacji dla całej lokacji. Ten samouczek przedstawia sposób, w jaki można utworzyć spójny wygląd i działanie na wszystkich stronach, które można łatwo zaktualizować.

## <a name="introduction"></a>Wprowadzenie

Jedną z typowych cech witryn sieci Web przyjaznych dla użytkownika jest to, że mają one spójny układ strony i schemat nawigacji dla całej lokacji. W systemie ASP.NET 2,0 wprowadzono dwie nowe funkcje, które znacznie upraszczają implementowanie zarówno układu strony w całej lokacji, jak i schematu nawigacji: stron wzorcowych i nawigowania po witrynie. Strony wzorcowe umożliwiają deweloperom tworzenie szablonu w całej lokacji z wydzielonymi regionami edytowalnymi. Ten szablon można następnie zastosować do stron ASP.NET w witrynie. Takie strony ASP.NET muszą udostępniać tylko zawartość dla określonych edytowalnych regionów strony głównej wszystkie inne adiustacje na stronie wzorcowej są identyczne we wszystkich stronach ASP.NET, które używają strony wzorcowej. Ten model umożliwia deweloperom Definiowanie i scentralizowanie układu strony w całej lokacji, co ułatwia tworzenie spójnego wyglądu i działania na wszystkich stronach, które można łatwo zaktualizować.

[System nawigacji lokacji](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) zapewnia zarówno mechanizmowi dla deweloperów stron Definiowanie mapy witryny, jak i interfejsu API dla tej mapy lokacji, aby można było programowo zbadać zapytanie. Nowe kontrolki sieci Web nawigacji menu, TreeView i ścieżki mapy witryny ułatwiają renderowanie całości lub części mapy witryny we wspólnym elemencie interfejsu użytkownika nawigacji. Będziemy używać domyślnego dostawcy nawigacji w witrynie, co oznacza, że nasze mapowanie witryny zostanie zdefiniowane w pliku w formacie XML.

Aby zilustrować te koncepcje i sprawić, że witryna sieci Web samouczków jest bardziej użyteczna, przyjrzyjmy się tej lekcji definiowania układu strony w całej lokacji, implementacji mapy lokacji i dodawaniu interfejsu użytkownika nawigacji. Po zakończeniu tego samouczka będziemy korzystać z dobrze zaprojektowanego projektu witryny sieci Web na potrzeby tworzenia stron internetowych samouczków.

[![wyniku końcowego tego samouczka](master-pages-and-site-navigation-cs/_static/image2.png)](master-pages-and-site-navigation-cs/_static/image1.png)

**Rysunek 1**. końcowy wynik tego samouczka ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-pages-and-site-navigation-cs/_static/image3.png))

## <a name="step-1-creating-the-master-page"></a>Krok 1. Tworzenie strony wzorcowej

Pierwszym krokiem jest utworzenie strony wzorcowej dla witryny. Teraz nasza witryna sieci Web składa się tylko z określonego zestawu danych (`Northwind.xsd`, w folderze `App_Code`) LOGIKI biznesowej klasy (`ProductsBLL.cs`, `CategoriesBLL.cs`i tak dalej, wszystkie w folderze `App_Code`), bazę danych (`NORTHWND.MDF`, w folderze `App_Data`), plik konfiguracji (`Web.config`) i plik arkusza stylów CSS (`Styles.css`). Po wyczyszczeniu tych stron i plików przedstawiających użycie DAL i LOGIKI biznesowej z pierwszych dwóch samouczków, ponieważ będziemy przebadali te przykłady bardziej szczegółowo w przyszłych samouczkach.

![Pliki w naszym projekcie](master-pages-and-site-navigation-cs/_static/image4.png)

**Rysunek 2**. pliki w naszym projekcie

Aby utworzyć stronę wzorcową, kliknij prawym przyciskiem myszy nazwę projektu w Eksplorator rozwiązań i wybierz polecenie Dodaj nowy element. Następnie wybierz typ strony głównej z listy szablonów i nadaj jej nazwę `Site.master`.

[![dodać nową stronę wzorcową do witryny sieci Web](master-pages-and-site-navigation-cs/_static/image6.png)](master-pages-and-site-navigation-cs/_static/image5.png)

**Rysunek 3**. Dodawanie nowej strony wzorcowej do witryny sieci Web ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-pages-and-site-navigation-cs/_static/image7.png))

W tym miejscu Zdefiniuj układ strony dla całej witryny na stronie wzorcowej. Możesz użyć widok Projekt i dodać wszelkie wymagane przez siebie kontrolki układu lub sieci Web lub ręcznie dodać znaczniki adiustacji do widoku źródła. Na mojej stronie wzorcowej są używane [kaskadowe arkusze stylów](http://www.w3schools.com/css/default.asp) do pozycjonowania i stylów z ustawieniami CSS zdefiniowanymi w pliku zewnętrznym `Style.css`. Chociaż nie możesz powiedzieć od znaczników przedstawionych poniżej, reguły CSS są zdefiniowane w taki sposób, że zawartość `<div>`nawigacji jest pozycjonowana absolutnie, tak aby była wyświetlana po lewej stronie i ma stałą szerokość 200 pikseli.

Site. Master

[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample1.aspx)]

Strona wzorcowa definiuje zarówno statyczny układ strony, jak i regiony, które mogą być edytowane przez strony ASP.NET, które korzystają z strony wzorcowej. Te regiony edytowalne zawartości są wskazywane przez formant ContentPlaceHolder, który może być widoczny w `<div>`zawartości. Nasza Strona główna ma jeden element ContentPlaceHolder (`MainContent`), ale strona wzorcowa może mieć wiele Elementy ContentPlaceHolders.

Po wprowadzeniu znacznika w widok Projekt zostanie wyświetlona strona wzorcowa. Wszystkie strony ASP.NET, które używają tej strony wzorcowej będą miały jednolity układ, z możliwością określenia znaczników dla regionu `MainContent`.

[![stronę wzorcową, gdy jest wyświetlany w widoku projektu](master-pages-and-site-navigation-cs/_static/image9.png)](master-pages-and-site-navigation-cs/_static/image8.png)

**Rysunek 4**. Strona wzorcowa wyświetlana w widoku projektu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-pages-and-site-navigation-cs/_static/image10.png))

## <a name="step-2-adding-a-homepage-to-the-website"></a>Krok 2. Dodawanie strony głównej do witryny sieci Web

Po zdefiniowaniu strony głównej wszystko jest gotowe do dodania stron ASP.NET dla witryny sieci Web. Zacznijmy od dodania `Default.aspx`, strony głównej witryny sieci Web. Kliknij prawym przyciskiem myszy nazwę projektu w Eksplorator rozwiązań i wybierz polecenie Dodaj nowy element. Wybierz opcję formularz sieci Web z listy szablon i nadaj nazwę plikowi `Default.aspx`. Zaznacz również pole wyboru "Wybierz stronę wzorcową".

[![dodać nowy formularz sieci Web, zaznaczając pole wyboru Wybierz stronę wzorcową](master-pages-and-site-navigation-cs/_static/image12.png)](master-pages-and-site-navigation-cs/_static/image11.png)

**Rysunek 5**. Dodaj nowy formularz sieci Web, zaznaczając pole wyboru Wybierz stronę wzorcową ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-pages-and-site-navigation-cs/_static/image13.png))

Po kliknięciu przycisku OK zostanie wyświetlona prośba o wybranie strony wzorcowej, która ma być używana przez nową stronę ASP.NET. Chociaż można mieć wiele stron wzorcowych w projekcie, mamy tylko jeden.

[![wybrać stronę wzorcową, która powinna być używana na tej stronie ASP.NET](master-pages-and-site-navigation-cs/_static/image15.png)](master-pages-and-site-navigation-cs/_static/image14.png)

**Ilustracja 6**. Wybierz stronę wzorcową, która powinna być używana na tej stronie ASP.NET ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-pages-and-site-navigation-cs/_static/image16.png))

Po wybraniu strony wzorcowej nowe strony ASP.NET będą zawierać następujące znaczniki:

Default. aspx

[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample2.aspx)]

W `@Page` dyrektywie istnieje odwołanie do pliku strony głównej używany (`MasterPageFile="~/Site.master"`), a znacznik strony ASP.NET zawiera kontrolkę zawartości dla każdej kontrolki elementu ContentPlaceHolder zdefiniowanej na stronie wzorcowej, przy czym `ContentPlaceHolderID` mapowania kontrolki zawartości na określony element ContentPlaceHolder. Kontrolka zawartości polega na umieszczeniu znacznika, który ma być wyświetlany w odpowiadającym mu elemencie ContentPlaceHolder. Ustaw atrybut `Title` dyrektywy `@Page` na Strona główna i Dodaj do kontrolki zawartość treść powitalną:

Default. aspx

[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample3.aspx)]

Atrybut `Title` w dyrektywie `@Page` pozwala na ustawienie tytułu strony na stronie ASP.NET, mimo że element `<title>` jest zdefiniowany na stronie wzorcowej. Można również programowo ustawić tytuł przy użyciu `Page.Title`. Należy również zauważyć, że odwołania do stron wzorcowych (takie jak `Style.css`) są automatycznie aktualizowane, tak aby działały na dowolnej stronie ASP.NET, niezależnie od tego, jaki katalog jest stroną ASP.NET względem strony głównej.

Przełączenie do widok Projekt może zobaczyć, jak wygląda strona w przeglądarce. Należy pamiętać, że w widok Projekt dla strony ASP.NET, że tylko edytowalne regiony zawartości są edytowane. znaczniki inne niż ContentPlaceHolder zdefiniowane na stronie wzorcowej są wyszarzone.

[![widok Projekt na stronie ASP.NET pokazuje zarówno regiony edytowalne, jak i nieedytowalne](master-pages-and-site-navigation-cs/_static/image18.png)](master-pages-and-site-navigation-cs/_static/image17.png)

**Rysunek 7**. Widok projektu na stronie ASP.net pokazuje zarówno edytowalne, jak i nieedytowalne regiony ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-pages-and-site-navigation-cs/_static/image19.png))

Gdy strona `Default.aspx` jest odwiedzana przez przeglądarkę, aparat ASP.NET automatycznie scala zawartość strony wzorcowej i stronę ASP. Zawartość sieci i renderuje scaloną zawartość w końcowym kodzie HTML, który jest wysyłany do przeglądarki żądającej. Po zaktualizowaniu zawartości strony wzorcowej wszystkie strony ASP.NET, które używają tej strony wzorcowej, zostaną ponownie scalone z nową stroną wzorcową przy następnym żądaniu. W skrócie modelu strony głównej można zdefiniować szablon układu pojedynczego strony (Strona główna), którego zmiany są natychmiast odzwierciedlane w całej lokacji.

## <a name="adding-additional-aspnet-pages-to-the-website"></a>Dodawanie dodatkowych stron ASP.NET do witryny sieci Web

Poświęć chwilę na dodanie dodatkowych ASP.NET stron do lokacji, która ostatecznie będzie zawierała różne pokazy raportowania. W sumie będzie dostępnych więcej niż 35 demonstracyjnych, więc zamiast tworzenia wszystkich stron zastępczych można utworzyć kilka pierwszych. Ponieważ istnieje również wiele kategorii pokazów, w celu lepszego zarządzania pokazami Dodaj folder dla kategorii. Dla tej pory Dodaj następujące trzy foldery:

- `BasicReporting`
- `Filtering`
- `CustomFormatting`

Na koniec Dodaj nowe pliki, jak pokazano na Eksplorator rozwiązań na rysunku 8. Podczas dodawania każdego pliku Pamiętaj, aby zaznaczyć pole wyboru "Wybierz stronę wzorcową".

![Dodaj następujące pliki](master-pages-and-site-navigation-cs/_static/image20.png)

**Ilustracja 8**. Dodawanie następujących plików

## <a name="step-2-creating-a-site-map"></a>Krok 2. Tworzenie mapy witryny

Jednym z wyzwań związanych z zarządzaniem witryną sieci Web składającą się z ponad kilku stron jest umożliwienie odwiedzającemu nawigowania po witrynie. Aby rozpocząć od, należy zdefiniować strukturę nawigacyjną lokacji. Następnie tę strukturę należy przetłumaczyć na elementy interfejsu użytkownika nawigacji, takie jak menu lub struktura nawigacyjna. Na koniec cały proces musi być konserwowany i aktualizowany po dodaniu nowych stron do lokacji i usunięciu istniejących. Przed ASP.NET 2,0, deweloperzy byli we własnym zakresie tworzyć strukturę nawigacyjną lokacji, utrzymywać ją i przetłumaczyć na elementy interfejsu użytkownika. W przypadku ASP.NET 2,0 deweloperzy mogą jednak korzystać z bardzo elastycznego systemu nawigacji w lokacji.

System nawigacji w witrynie ASP.NET 2,0 zapewnia deweloperowi możliwość zdefiniowania mapy witryny i uzyskania dostępu do tych informacji za pośrednictwem programu programistycznego interfejsu API. ASP.NET jest dostarczany z dostawcą mapy witryny, który oczekuje, że dane mapy witryny mają być przechowywane w pliku XML sformatowanym w określony sposób. Jednak ponieważ system nawigacji lokacji jest oparty na [modelu dostawcy](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) , można go rozszerzyć, aby obsługiwał alternatywne sposoby serializowania informacji z mapy witryny. W artykule Jan Prosise [dostawca mapy witryny SQL, dla którego oczekujesz](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx) , pokazuje, jak utworzyć dostawcę mapy witryny przechowującego mapę lokacji w bazie danych SQL Server. innym rozwiązaniem jest utworzenie [dostawcy mapy witryny opartego na strukturze systemu plików](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx).

Na potrzeby tego samouczka użyjemy domyślnego dostawcy mapy witryny, który jest dostarczany z ASP.NET 2,0. Aby utworzyć mapę witryny, po prostu kliknij prawym przyciskiem myszy nazwę projektu w Eksplorator rozwiązań, wybierz polecenie Dodaj nowy element i wybierz opcję Mapa witryny. Pozostaw nazwę jako `Web.sitemap` i kliknij przycisk Dodaj.

[![dodać mapę witryny do projektu](master-pages-and-site-navigation-cs/_static/image22.png)](master-pages-and-site-navigation-cs/_static/image21.png)

**Ilustracja 9**. Dodawanie mapy witryny do projektu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-pages-and-site-navigation-cs/_static/image23.png))

Plik mapy witryny jest plikiem XML. Należy zauważyć, że program Visual Studio udostępnia funkcję IntelliSense dla struktury mapy witryny. Plik mapy witryny musi mieć węzeł `<siteMap>` jako węzeł główny, który musi zawierać dokładnie jeden `<siteMapNode>` element podrzędny. Ten pierwszy `<siteMapNode>` element może następnie zawierać dowolną liczbę elementów potomnych `<siteMapNode>`.

Zdefiniuj mapę witryny, aby naśladować strukturę systemu plików. Oznacza to, że należy dodać element `<siteMapNode>` dla każdego z trzech folderów i elementów podrzędnych `<siteMapNode>` dla każdej z ASP.NET stron w tych folderach, tak jak to zrobić:

Web. sitemap

[!code-xml[Main](master-pages-and-site-navigation-cs/samples/sample4.xml)]

Mapa witryny definiuje strukturę nawigacyjną witryny sieci Web, która jest hierarchią opisującą różne sekcje witryny. Każdy element `<siteMapNode>` w `Web.sitemap` reprezentuje sekcję w strukturze nawigacyjnej lokacji.

[![mapa witryny reprezentuje hierarchiczną strukturę nawigacyjną](master-pages-and-site-navigation-cs/_static/image25.png)](master-pages-and-site-navigation-cs/_static/image24.png)

**Ilustracja 10**. Mapa lokacji reprezentuje hierarchiczną strukturę nawigacyjną ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-pages-and-site-navigation-cs/_static/image26.png))

ASP.NET uwidacznia strukturę mapy witryny za pomocą [klasy SiteMap](https://msdn.microsoft.com/library/system.web.sitemap.aspx).NET Framework. Ta klasa ma właściwość `CurrentNode`, która zwraca informacje o sekcji, do której użytkownik jest aktualnie odwiedzany; Właściwość `RootNode` zwraca katalog główny mapy witryny (Strona główna w naszym mapie witryny). Właściwości `CurrentNode` i `RootNode` zwracają wystąpienia [SiteMapNode](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) , które mają właściwości, takie jak `ParentNode`, `ChildNodes`, `NextSibling`, `PreviousSibling`i tak dalej, które zezwalają na przechodzenie hierarchii mapy witryny.

## <a name="step-3-displaying-a-menu-based-on-the-site-map"></a>Krok 3. Wyświetlanie menu w oparciu o mapę witryny

Dostęp do danych w programie ASP.NET 2,0 można wykonać programowo, jak w ASP.NET 1. x, lub deklaratywnie, za pomocą nowych [kontrolek źródła danych](https://msdn.microsoft.com/library/ms227679.aspx). Istnieje kilka wbudowanych formantów źródła danych, takich jak kontrolka kontrolki SqlDataSource, do uzyskiwania dostępu do relacyjnych danych bazy danych, kontrolki ObjectDataSource, do uzyskiwania dostępu do danych z klas i innych. Można nawet utworzyć własne [niestandardowe kontrolki źródła danych](https://msdn.microsoft.com/asp.net/reference/data/default.aspx?pull=/library/dnvs05/html/DataSourceCon1.asp).

Kontrolki źródła danych służy jako serwer proxy między stroną ASP.NET a danymi źródłowymi. Aby można było wyświetlić pobrane dane kontroli źródła danych, zwykle dodamy kolejną kontrolkę sieci Web do strony i powiążesz ją z kontrolką źródła danych. Aby powiązać formant sieci Web z kontrolką źródła danych, wystarczy ustawić właściwość `DataSourceID` kontrolki sieci Web na wartość właściwości `ID` kontrolki źródła danych.

Aby pomóc w pracy z danymi mapy witryny, ASP.NET zawiera kontrolkę SiteMapDataSource, która pozwala na powiązanie formantu internetowego z mapą witryny witryny sieci Web. Dwie kontrolki sieci Web widok TreeView i menu są często używane do udostępniania interfejsu użytkownika nawigacji. Aby powiązać dane mapy lokacji z jedną z tych dwóch kontrolek, wystarczy dodać SiteMapDataSource do strony wraz z kontrolką TreeView lub menu, której Właściwość `DataSourceID` jest odpowiednio ustawiona. Na przykład możemy dodać formant menu do strony wzorcowej, korzystając z następującej znaczników:

[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample5.aspx)]

Aby zapewnić bardziej precyzyjną kontrolę nad emitowanym kodem HTML, możemy powiązać formant SiteMapDataSource z kontrolką wzmacniania, np.:

[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample6.aspx)]

Kontrolka SiteMapDataSource zwraca hierarchię mapy witryny po jednym poziomie naraz, rozpoczynając od węzła mapy witryny głównej (dom, na naszej mapie witryny), a następnie na następnym poziomie (raportowanie podstawowe, filtrowanie raportów i dostosowane formatowanie) itd. W przypadku powiązania SiteMapDataSource z Wzmacniaką wylicza pierwszy zwrócony poziom i tworzy wystąpienie `ItemTemplate` dla każdego wystąpienia `SiteMapNode` w tym pierwszym poziomie. Aby uzyskać dostęp do określonej właściwości `SiteMapNode`, możemy użyć `Eval(propertyName)`, co oznacza, że wszystkie `SiteMapNode``Url` i `Title` właściwości kontrolki HyperLink.

Powyższy przykład wzmacniania będzie renderować następujący znacznik:

[!code-html[Main](master-pages-and-site-navigation-cs/samples/sample7.html)]

Te węzły mapy witryny (raportowanie podstawowe, filtrowanie raportów i dostosowane formatowanie) składają się na *drugi* poziom renderowanej mapy witryny, a nie jako pierwszy. Wynika to z faktu, że właściwość `ShowStartingNode` SiteMapDataSource ma wartość false, co powoduje, że SiteMapDataSource pomija węzeł mapy witryny głównej, a zamiast tego zaczyna się od zwrócenia drugiego poziomu w hierarchii mapy lokacji.

Aby wyświetlić elementy podrzędne dla podstawowego raportowania, filtrowania raportów i niestandardowego formatowania `SiteMapNode` s, możemy dodać kolejny wzmacniak do `ItemTemplate`pierwszego Wzmacniakka. Ten drugi wzmacniak zostanie powiązany z właściwością `ChildNodes` `SiteMapNode` wystąpienia, na przykład:

[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample8.aspx)]

Te dwa powtórzenia powodują następujące znaczniki (niektóre adiustacje zostały usunięte dla zwięzłości):

[!code-html[Main](master-pages-and-site-navigation-cs/samples/sample9.html)]

Przy użyciu stylów CSS wybranych z książki [Rachel Andrew](http://www.rachelandrew.co.uk/) [Anthology: 101 podstawowe wskazówki, sztuczki, &amp; Hacks](https://www.amazon.com/gp/product/0957921888/qid=1137565739/sr=8-1/ref=pd_bbs_1/103-0562306-3386214?n=507846&amp;s=books&amp;v=glance), elementy `<ul>` i `<li>` są wzorowane tak, że znaczniki da następujące wizualizacje wyjściowe:

![Menu składające się z dwóch powtórzeń i niektórych stylów CSS](master-pages-and-site-navigation-cs/_static/image27.png)

**Ilustracja 11**. menu składające się z dwóch powtórzeń i niektórych stylów CSS

To menu znajduje się na stronie wzorcowej i jest powiązane z mapą witryny zdefiniowaną w `Web.sitemap`, co oznacza, że wszelkie zmiany mapy witryny zostaną natychmiast odzwierciedlone na wszystkich stronach, które używają `Site.master` stronie wzorcowej.

## <a name="disabling-viewstate"></a>Wyłączanie stanu wyświetlania

Wszystkie kontrolki ASP.NET mogą opcjonalnie utrzymywać swój stan w [stanie widoku](https://msdn.microsoft.com/msdnmag/issues/03/02/CuttingEdge/), który jest serializowany jako ukryte pole formularza w RENDEROWANYM kodzie HTML. Stan widoku jest używany przez kontrolki do zapamiętania ich stanu programistycznie zmieniony w ramach ogłaszania zwrotnego, takich jak dane powiązane z formantem sieci Web danych. W trakcie wyświetlania stanu informacje, które mogą być zapamiętywane przez ogłaszanie zwrotne, zwiększają rozmiar znacznika, który musi zostać wysłany do klienta i może prowadzić do poważnej przeładowanie strony, jeśli nie jest ściśle monitorowany. Kontrolki sieci Web danych szczególnie GridView są szczególnie Notorious do dodawania dziesiątek dodatkowych kilobajtów znaczników do strony. Chociaż taki wzrost może być nieznaczny dla użytkowników szerokopasmowych lub intranetowych, stan widoku może dodawać kilka sekund do rundy dla użytkowników połączeń telefonicznych.

Aby zobaczyć wpływ stanu widoku, odwiedź stronę w przeglądarce, a następnie Wyświetl źródło wysyłane przez stronę sieci Web (w programie Internet Explorer przejdź do menu Widok i wybierz opcję Źródło). Możesz również włączyć [śledzenie stron](https://msdn.microsoft.com/library/sfbfw58f.aspx) , aby zobaczyć przydział stanu widoku używany przez poszczególne kontrolki na stronie. Informacje o stanie widoku są serializowane w ukrytym polu formularza o nazwie `__VIEWSTATE`, które znajdują się w elemencie `<div>` bezpośrednio po tagu otwierającym `<form>`. Stan widoku jest utrwalany tylko wtedy, gdy jest używany formularz sieci Web; Jeśli strona ASP.NET nie zawiera `<form runat="server">` w swojej składni deklaracyjnej, nie będzie `__VIEWSTATE` ukrytym polem formularza w renderowanej adjustacji.

Pole formularza `__VIEWSTATE` wygenerowane przez stronę wzorcową dodaje około 1 800 bajtów do wygenerowanego znacznika strony. Ten dodatkowy przeładowanie jest przeznaczony głównie dla formantu wzmacniania, ponieważ zawartość kontrolki SiteMapDataSource są utrwalane w celu wyświetlenia stanu. Mimo że dodatkowe 1 800 bajty mogą nie pozornie przyjemnością się o to, gdy korzystasz z widoku GridView z wieloma polami i rekordami, stan widoku można łatwo Swell przez czynnik 10 lub więcej.

Stan widoku można wyłączyć na stronie lub na poziomie kontroli przez ustawienie właściwości `EnableViewState` na `false`, co zmniejsza rozmiar renderowanego znacznika. Ponieważ stan widoku dla kontrolki sieci Web danych utrzymuje dane powiązane z formantem sieci Web danych przez ogłaszanie zwrotne, w przypadku wyłączenia stanu widoku dla kontrolki sieci Web danych muszą one być powiązane z każdym i każdym ogłaszaniem zwrotnym. W ASP.NET w wersji 1. x ta odpowiedzialność spadła w przypadku twórców stron. w przypadku ASP.NET 2,0, w razie potrzeby, formanty sieci Web danych zostaną ponownie powiązane z kontrolą źródła danych na każdym ogłaszaniu zwrotnym.

Aby zmniejszyć stan widoku strony, ustaw właściwość `EnableViewState` formantu wzmacniak na `false`. Można to zrobić za pomocą okno Właściwości w projektancie lub deklaratywnie w widoku źródła. Po wprowadzeniu tej zmiany deklaracyjne znaczniki powinny wyglądać następująco:

[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample10.aspx)]

Po tej zmianie rozmiar renderowanego widoku strony został zmniejszony do zwykłego 52 bajtów, a 97% oszczędności w rozmiarze stanu widoku! W samouczkach w tej serii wyłączysz stan widoku kontrolek sieci Web danych domyślnie, aby zmniejszyć rozmiar renderowanego znacznika. We wszystkich przykładach Właściwość `EnableViewState` zostanie ustawiona na `false` i wykonana bez wzmianki. Jedyny stan widoku czasu zostanie omówiony w scenariuszach, w których musi być włączony, aby formant sieci Web danych mógł zapewnić jego oczekiwaną funkcjonalność.

## <a name="step-4-adding-breadcrumb-navigation"></a>Krok 4. Dodawanie nawigacji nawigacyjnej

Aby ukończyć stronę wzorcową, Dodaj do każdej strony element interfejsu użytkownika nawigacji do stron nadrzędnych. Na pasku nawigacyjnym szybko są wyświetlane bieżące położenie użytkowników w hierarchii lokacji. Dodawanie stron nadrzędnych w programie ASP.NET 2,0 to łatwe dodawanie kontrolki ścieżki mapy witryny do strony; kod nie jest wymagany.

W naszej witrynie Dodaj tę kontrolkę do nagłówka `<div>`:

[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample11.aspx)]

Pasek nawigacyjny pokazuje bieżącą stronę, którą odwiedza użytkownik w hierarchii mapy witryny, a także ten węzeł mapy witryny "elementy nadrzędne", tak jak w przypadku wszystkich elementów głównych (domowych, na naszej mapie witryny).

![Pasek nawigacyjny wyświetla bieżącą stronę i jej elementy nadrzędne w hierarchii mapy witryny](master-pages-and-site-navigation-cs/_static/image28.png)

**Ilustracja 12**. pasek nawigacyjny wyświetla bieżącą stronę i jej elementy nadrzędne w hierarchii mapy lokacji

## <a name="step-5-adding-the-default-page-for-each-section"></a>Krok 5. Dodawanie strony domyślnej dla każdej sekcji

Samouczki w naszej witrynie są podzielone na różne kategorie podstawowe raporty, filtrowanie, formatowanie niestandardowe i tak dalej, z folderem dla każdej kategorii i odpowiednimi samouczkami jako ASP.NET strony w tym folderze. Ponadto każdy folder zawiera stronę `Default.aspx`. Na tej stronie domyślnej zostanie wyświetlone wszystkie samouczki dla bieżącej sekcji. Oznacza to, że dla `Default.aspx` w folderze `BasicReporting` mamy linki do `SimpleDisplay.aspx`, `DeclarativeParams.aspx`i `ProgrammaticParams.aspx`. W tym miejscu można ponownie użyć klasy `SiteMap` i kontrolki sieci Web danych, aby wyświetlić te informacje na podstawie mapy witryny zdefiniowanej w `Web.sitemap`.

Wyświetlmy listę nieuporządkowaną ponownie przy użyciu przewodnika, ale ten czas będzie wyświetlał tytuł i opis samouczków. Ponieważ adiustacje i kod do osiągnięcia należy powtórzyć dla każdej strony `Default.aspx`, możemy hermetyzować tę logikę interfejsu [użytkownika w kontrolce](https://msdn.microsoft.com/library/y6wb1a0e.aspx). Utwórz folder w witrynie sieci Web o nazwie `UserControls` i Dodaj do niego nowy element typ kontrolki użytkownika sieci Web o nazwie `SectionLevelTutorialListing.ascx`i Dodaj następujący znacznik:

[![dodać nową kontrolkę użytkownika sieci Web do folderu UserControls](master-pages-and-site-navigation-cs/_static/image30.png)](master-pages-and-site-navigation-cs/_static/image29.png)

**Ilustracja 13**. Dodawanie nowej kontrolki użytkownika sieci Web do folderu `UserControls` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-site-navigation-cs/_static/image31.png))

SectionLevelTutorialListing. ascx

[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample12.aspx)]

SectionLevelTutorialListing.ascx.cs

[!code-csharp[Main](master-pages-and-site-navigation-cs/samples/sample13.cs)]

W poprzednim przykładzie wzmacniak powiązana z `SiteMap` danymi do wzmacniaka deklaratywnie; jednak kontrolka użytkownika `SectionLevelTutorialListing`. W obsłudze zdarzeń `Page_Load` jest wykonywane sprawdzenie, aby zapewnić, że ten adres URL strony jest mapowany na węzeł na mapie witryny. Jeśli ta kontrolka użytkownika jest używana na stronie, która nie ma odpowiedniego wpisu `<siteMapNode>`, `SiteMap.CurrentNode` zwróci `null` i żadne dane nie zostaną powiązane z tym Wzmacniakem. Przy założeniu, że mamy `CurrentNode`, powiążemy swoją `ChildNodes`ą kolekcję z Wzmacniaką. Ponieważ nasza mapa witryny jest skonfigurowana tak, że strona `Default.aspx` w każdej sekcji jest węzłem nadrzędnym wszystkich samouczków w tej sekcji, ten kod będzie wyświetlał linki do i opisy wszystkich samouczków sekcji, jak pokazano na poniższym zrzucie ekranu.

Po utworzeniu tego wzmacniaka Otwórz `Default.aspx` strony w każdym z folderów, przejdź do widok Projekt i po prostu przeciągnij kontrolkę użytkownika z Eksplorator rozwiązań na powierzchnię projektu, w której ma zostać wyświetlona lista samouczków.

[![kontrolka użytkownika została dodana do default. aspx](master-pages-and-site-navigation-cs/_static/image33.png)](master-pages-and-site-navigation-cs/_static/image32.png)

**Ilustracja 14**. kontrolka użytkownika została dodana do `Default.aspx` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-pages-and-site-navigation-cs/_static/image34.png))

[![podstawowe samouczki dotyczące raportowania znajdują się na liście](master-pages-and-site-navigation-cs/_static/image36.png)](master-pages-and-site-navigation-cs/_static/image35.png)

**Ilustracja 15**. wyświetlane są podstawowe samouczki dotyczące raportowania ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-pages-and-site-navigation-cs/_static/image37.png))

## <a name="summary"></a>Podsumowanie

Po zdefiniowaniu mapy witryny i zakończeniu strony wzorcowej mamy teraz spójny układ strony i schemat nawigacji dla naszych samouczków związanych z danymi. Niezależnie od tego, ile stron dodaliśmy do naszej witryny, aktualizowanie układu strony w całej lokacji lub informacji o nawigacji lokacji jest szybkim i prostym procesem z powodu scentralizowanych informacji. Informacje o układzie strony są definiowane na stronie głównej `Site.master` a Mapa lokacji w `Web.sitemap`. Nie musimy pisać *żadnego* kodu w celu osiągnięcia tego układu strony i mechanizmu nawigacji dla całej witryny, a my zachowujemy pełną obsługę funkcji WYSIWYG Designer w programie Visual Studio.

Po ukończeniu warstwy dostępu do danych i warstwy logiki biznesowej oraz mającej zdefiniowany spójny układ strony i nawigację w witrynie wszystko jest gotowe do rozpoczęcia eksplorowania wspólnych wzorców raportowania. W [następnych](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) trzech samouczkach zaobserwujemy podstawowe zadania raportowania zawierające dane pobrane z logiki biznesowej w kontrolkach GridView, DetailsView i FormView.

Szczęśliwe programowanie!

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [ASP.NET strony wzorcowe — Omówienie](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [Strony wzorcowe w ASP.NET 2,0](http://odetocode.com/Articles/419.aspx)
- [Szablony projektów w programie ASP.NET 2,0](https://msdn.microsoft.com/asp.net/reference/design/templates/default.aspx)
- [Nawigacja w witrynie ASP.NET — Omówienie](https://msdn.microsoft.com/library/e468hxky.aspx)
- [Badanie nawigacji w witrynie ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Funkcje nawigowania po witrynie ASP.NET 2,0](https://weblogs.asp.net/scottgu/archive/2005/11/20/431019.aspx)
- [Informacje o stanie widoku ASP.NET](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/viewstate.asp)
- [Instrukcje: Włączanie śledzenia dla strony ASP.NET](https://msdn.microsoft.com/library/94c55d08%28VS.80%29.aspx)
- [Kontrolki użytkownika ASP.NET](https://msdn.microsoft.com/library/y6wb1a0e.aspx)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci liderzy dla tego samouczka to Liz Shulok, Dennis Patterson i Hilton Giesenow. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](creating-a-business-logic-layer-cs.md)
> [dalej](creating-a-data-access-layer-vb.md)

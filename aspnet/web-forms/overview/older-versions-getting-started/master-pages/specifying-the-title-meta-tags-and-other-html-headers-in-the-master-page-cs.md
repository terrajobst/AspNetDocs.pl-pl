---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs
title: Określanie tytułu, tagów Meta i innych nagłówków HTML na stronie wzorcowej (C#) | Microsoft Docs
author: rick-anderson
description: Analizuje różne techniki definiowania elementów &lt;&gt; na stronie wzorcowej ze strony zawartość.
ms.author: riande
ms.date: 05/21/2008
ms.assetid: 0aa1c84f-c9e2-4699-b009-0e28643ecbc6
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs
msc.type: authoredcontent
ms.openlocfilehash: a4ec96a5b90f664655d554c064f9d50e76ad2d58
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78587109"
---
# <a name="specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-c"></a>Określanie tytułu, tagów meta i innych nagłówków HTML na stronie wzorcowej (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_03_CS.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_03_CS.pdf)

> Analizuje różne techniki definiowania elementów &lt;&gt; na stronie wzorcowej ze strony zawartość.

## <a name="introduction"></a>Wprowadzenie

Nowe strony wzorcowe utworzone w programie Visual Studio 2008 mają domyślnie dwie kontrolki ContentPlaceHolder: jeden nazwany nagłówek i znajduje się w elemencie `<head>`; i jeden nazwany `ContentPlaceHolder1`, umieszczony w formularzu sieci Web. Celem `ContentPlaceHolder1` jest zdefiniowanie regionu w formularzu sieci Web, który można dostosować w zależności od strony. `head` ContentPlaceHolder umożliwia stronom Dodawanie zawartości niestandardowej do sekcji `<head>`. (Oczywiście te dwa elementy ContentPlaceHolder można modyfikować lub usuwać, a do strony głównej można dodawać dodatkowe elementy ContentPlaceHolder. Nasza Strona główna, `Site.master`, ma obecnie cztery kontrolki ContentPlaceHolder.)

Element `<head>` HTML służy jako repozytorium informacji o dokumencie strony sieci Web, który nie jest częścią samego dokumentu. Obejmuje to takie informacje jak tytuł strony sieci Web, meta-informacje używane przez aparaty wyszukiwania lub wewnętrzne przeszukiwania oraz linki do zasobów zewnętrznych, takich jak źródła danych RSS, JavaScript i pliki CSS. Niektóre z tych informacji mogą dotyczyć wszystkich stron w witrynie sieci Web. Na przykład możesz chcieć zaimportować globalne te same reguły CSS i pliki JavaScript dla każdej strony ASP.NET. Istnieją jednak części `<head>` elementu, które są specyficzne dla strony. Tytuł strony jest głównym przykładem.

W tym samouczku opisano sposób definiowania znaczników globalnych i specyficznych dla strony `<head>` na stronie wzorcowej i na stronach zawartości.

## <a name="examining-the-master-pagesheadsection"></a>Badanie sekcji`<head>`strony wzorcowej

Domyślny plik strony głównej utworzony przez program Visual Studio 2008 zawiera następujące znaczniki w sekcji `<head>`:

[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample1.aspx)]

Zauważ, że element `<head>` zawiera atrybut `runat="server"`, który wskazuje, że jest to formant serwera (zamiast statycznego kodu HTML). Wszystkie strony ASP.NET pochodzą z [klasy`Page`](https://msdn.microsoft.com/library/system.web.ui.page.aspx), która znajduje się w przestrzeni nazw `System.Web.UI`. Ta klasa zawiera właściwość `Header`, która zapewnia dostęp do regionu `<head>` strony. Za pomocą [właściwości`Header`](https://msdn.microsoft.com/library/system.web.ui.page.header.aspx) można ustawić tytuł strony ASP.NET lub dodać dodatkowe znaczniki do renderowanej `<head>` sekcji. Istnieje możliwość, że w celu dostosowania elementu `<head>` strony zawartości przez napisanie bitu kodu w programie obsługi zdarzeń `Page_Load` strony. Sprawdzimy, jak programowo ustawić tytuł strony w kroku 1.

Znaczniki wyświetlane w powyższym elemencie `<head>` zawierają również formant ContentPlaceHolder o nazwie nagłówek. Ta kontrolka ContentPlaceHolder nie jest konieczna, ponieważ strony zawartości mogą programowo dodać zawartość niestandardową do elementu `<head>`. Jest to przydatne, jednak w sytuacjach, gdy strona zawartości musi dodać znacznik statyczny do `<head>` elementu, ponieważ oznakowanie statyczne może zostać dodane deklaratywnie do odpowiedniej kontrolki zawartości zamiast programowo.

Oprócz elementu `<title>` i jego elementu ContentPlaceHolder element `<head>` strony wzorcowej powinien zawierać wszystkie znaczniki `<head>`na poziomie, które są wspólne dla wszystkich stron. W naszej witrynie sieci Web wszystkie strony używają reguł CSS zdefiniowanych w pliku `Styles.css`. W związku z tym Zaktualizowaliśmy element `<head>` w oknie [*Tworzenie układu dla całej witryny ze stronami wzorcowymi*](creating-a-site-wide-layout-using-master-pages-cs.md) w celu uwzględnienia odpowiedniego elementu `<link>`. Poniżej przedstawiono `Site.master` bieżącą `<head>`ą stronę wzorcową.

[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample2.aspx)]

## <a name="step-1-setting-a-content-pages-title"></a>Krok 1. Ustawianie tytułu strony zawartości

Tytuł strony sieci Web jest określany za pośrednictwem elementu `<title>`. Ważne jest, aby ustawić tytuł każdej strony na odpowiednią wartość. Podczas odwiedzania strony tytuł zostanie wyświetlony na pasku tytułu przeglądarki. Ponadto podczas zakładania zakładki strony przeglądarki używają tytułu strony jako sugerowanej nazwy zakładki. Ponadto wiele aparatów wyszukiwania Wyświetla tytuł strony podczas wyświetlania wyników wyszukiwania.

> [!NOTE]
> Domyślnie program Visual Studio ustawia element `<title>` na stronie wzorcowej na "stronę bez tytułu". Podobnie nowe strony ASP.NET `<title>` mają ustawioną wartość "Strona bez tytułu". Ze względu na to, że można łatwo zapomnieć, aby ustawić odpowiednią wartość tytułu strony, istnieje wiele stron w Internecie z tytułem "Strona niezatytułowana". Wyszukiwanie stron Google for Web z tym tytułem zwraca około 2 460 000 wyników. Nawet firma Microsoft jest podatna na publikowanie stron sieci Web z tytułem "Strona bez tytułu". W momencie tego zapisu w usłudze Google Search zgłoszono 236 takich stron sieci Web w domenie Microsoft.com.

Strona ASP.NET może określać jej tytuł w jeden z następujących sposobów:

- Przez umieszczenie wartości bezpośrednio w `<title>` elemencie
- Używanie `Title` atrybutu w dyrektywie `<%@ Page %>`
- Programowe Ustawianie właściwości `Title` strony przy użyciu kodu, takiego jak `Page.Title="title"` lub `Page.Header.Title="title"`.

Strony zawartości nie mają elementu `<title>`, ponieważ jest on zdefiniowany na stronie wzorcowej. W związku z tym, aby ustawić tytuł strony zawartości, można użyć atrybutu `Title` dyrektywy `<%@ Page %>` lub programowo ustawić go.

### <a name="setting-the-pages-title-declaratively"></a>Deklaratywne Ustawianie tytułu strony

Tytuł strony zawartości można ustawić deklaratywnie poprzez atrybut `Title` [dyrektywy`<%@ Page %>`](https://msdn.microsoft.com/library/ydy4x04a.aspx). Tę właściwość można ustawić przez bezpośrednie zmodyfikowanie dyrektywy `<%@ Page %>` lub przez okno Właściwości. Przyjrzyjmy się obu podejściem.

W widoku źródła Znajdź dyrektywę `<%@ Page %>`, która znajduje się w górnej części deklaracyjnego znacznika strony. `<%@ Page %>` dyrektywie `Default.aspx` poniżej:

[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample3.aspx)]

Dyrektywa `<%@ Page %>` określa atrybuty specyficzne dla strony używane przez aparat ASP.NET podczas analizowania i kompilowania strony. Obejmuje to plik strony głównej, lokalizację pliku kodu oraz jego tytuł, między innymi informacjami.

Domyślnie podczas tworzenia nowej strony zawartości program Visual Studio ustawia atrybut `Title` na stronę bez tytułu. Zmień atrybut `Title` `Default.aspx`z "strony" bez tytułu "na" samouczki stron wzorcowych ", a następnie Wyświetl stronę za pośrednictwem przeglądarki. Rysunek 1 pokazuje pasek tytułu przeglądarki, który odzwierciedla nowy tytuł strony.

![Na pasku tytułu przeglądarki są teraz wyświetlane &quot;samouczków strony wzorcowej&quot; zamiast &quot;strony bez tytułu&quot;](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image1.png)

**Ilustracja 01**. pasek tytułu przeglądarki pokazuje teraz "samouczki stron wzorcowych" zamiast "Strona bez tytułu"

Tytuł strony może być również ustawiony na podstawie okno Właściwości. Z okno Właściwości wybierz pozycję dokument z listy rozwijanej, aby załadować właściwości na poziomie strony, które obejmują Właściwość `Title`. Rysunek 2 przedstawia okno Właściwości po ustawieniu `Title` jako "samouczków strony wzorcowej".

![Możesz również skonfigurować tytuł z okna właściwości.](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image2.png)

**Ilustracja 02**. Możesz również skonfigurować tytuł z okna właściwości.

### <a name="setting-the-pages-title-programmatically"></a>Programistyczne Ustawianie tytułu strony

Znacznik `<head runat="server">` strony wzorcowej jest tłumaczony na wystąpienie [klasy`HtmlHead`](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.aspx) , gdy strona jest renderowana przez aparat ASP.NET. Klasa `HtmlHead` ma [właściwość`Title`](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.title.aspx) , której wartość jest odzwierciedlona w renderowanym elemencie `<title>`. Ta właściwość jest dostępna z klasy ASP.NET strony powiązanej z kodem za pośrednictwem `Page.Header.Title`; Tę samą Właściwość można również uzyskać za pośrednictwem `Page.Title`.

Aby programowo ustawić tytuł strony, przejdź do klasy związanej z kodem `About.aspx` strony i Utwórz procedurę obsługi zdarzeń dla zdarzenia `Load` strony. Następnie ustaw tytuł strony na "samouczki strony wzorcowej:: informacje:: *Data*", gdzie *Data* jest bieżącą datą. Po dodaniu tego kodu program obsługi zdarzeń `Page_Load` powinien wyglądać podobnie do poniższego:

[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample4.cs)]

Rysunek 3 pokazuje pasek tytułu przeglądarki podczas odwiedzania strony `About.aspx`.

![Tytuł strony jest programowo ustawiany i zawiera bieżącą datę](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image3.png)

**Ilustracja 03**: nazwa strony jest programowo ustawiana i zawiera bieżącą datę

## <a name="step-2-automatically-assigning-a-page-title"></a>Krok 2: automatyczne przypisywanie tytułu strony

Zgodnie z opisem w kroku 1, tytuł strony może być ustawiony deklaratywnie lub programowo. Jeśli zapomnisz jawnie zmienić tytuł na coś bardziej opisowego, strona będzie mieć tytuł domyślny "Strona bez tytułu". W idealnym przypadku tytuł strony zostanie automatycznie ustawiony dla nas w przypadku, gdy nie określimy jawnie jej wartości. Na przykład jeśli w środowisku uruchomieniowym tytuł strony jest "stroną bez tytułu", możemy chcieć, aby tytuł był automatycznie aktualizowany, tak samo jak nazwa pliku strony ASP.NET. Dobrą wiadomość polega na tym, że z małą ilością pracy z góry jest możliwe, że tytuł jest automatycznie przypisywany.

Wszystkie strony sieci Web ASP.NET pochodzą z klasy `Page` w przestrzeni nazw `System.Web.UI`. Klasa `Page` definiuje minimalną funkcjonalność wymaganą przez stronę ASP.NET i uwidacznia podstawowe właściwości, takie jak `IsPostBack`, `IsValid`, `Request`i `Response`, wśród wielu innych. Często, każda Strona w aplikacji sieci Web wymaga dodatkowych funkcji lub funkcjonalności. Typowym sposobem na zapewnienie tego jest utworzenie niestandardowej klasy strony podstawowej. Klasa niestandardowej strony podstawowej jest klasą utworzoną przez użytkownika, która dziedziczy z klasy `Page` i zawiera dodatkowe funkcje. Po utworzeniu tej klasy bazowej można uzyskać od niej strony ASP.NET (a nie klasy `Page`), oferując w ten sposób rozszerzoną funkcjonalność na stronach ASP.NET.

W tym kroku utworzymy stronę bazową, która automatycznie ustawia tytuł strony na nazwę pliku strony ASP.NET, jeśli tytuł nie został jawnie ustawiony. Krok 3 sprawdza, jak ustawić tytuł strony na podstawie mapy witryny.

> [!NOTE]
> Dokładne badanie tworzenia i używania niestandardowych klas stron podstawowych wykracza poza zakres tej serii samouczków. Aby uzyskać więcej informacji, Przeczytaj [przy użyciu niestandardowej klasy bazowej dla klas ASP.NET stron związanych z kodem](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx).

### <a name="creating-the-base-page-class"></a>Tworzenie klasy podstawowej strony

Pierwsze zadanie polega na utworzeniu klasy strony podstawowej, która jest klasą, która rozszerza klasę `Page`. Zacznij od dodania folderu `App_Code` do projektu, klikając prawym przyciskiem myszy nazwę projektu w Eksplorator rozwiązań, wybierając pozycję Dodaj folder ASP.NET, a następnie wybierając pozycję `App_Code`. Następnie kliknij prawym przyciskiem myszy folder `App_Code` i Dodaj nową klasę o nazwie `BasePage.cs`. Rysunek 4 przedstawia Eksplorator rozwiązań po dodaniu folderu `App_Code` i klasy `BasePage.cs`.

![Dodawanie folderu App_Code i klasy o nazwie BasePage](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image4.png)

**Ilustracja 04**: dodawanie folderu `App_Code` i klasy o nazwie `BasePage`

> [!NOTE]
> Program Visual Studio obsługuje dwa tryby zarządzania projektami: projekty witryny sieci Web i projekty aplikacji sieci Web. Folder `App_Code` jest przeznaczony do użycia z modelem projektu witryny sieci Web. Jeśli używasz modelu projektu aplikacji sieci Web, umieść klasę `BasePage.cs` w folderze o nazwie coś innego niż `App_Code`, na przykład `Classes`. Aby uzyskać więcej informacji na ten temat, zapoznaj się z tematem [Migrowanie projektu witryny sieci Web do projektu aplikacji sieci Web](http://webproject.scottgu.com/CSharp/Migration2/Migration2.aspx).

Ze względu na to, że niestandardowa strona podstawowa służy jako klasa bazowa dla klas ASP.NET stron z kodem, musi ona zostać rozszerzona o klasę `Page`.

[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample5.cs)]

Za każdym razem, gdy żądanie strony ASP.NET przechodzi przez serię etapów, culminating na żądana stronie jest renderowany w formacie HTML. Możemy wybrać etap, zastępując metodę `OnEvent` klasy `Page`. W przypadku naszej strony podstawowej automatycznie ustawimy tytuł, jeśli nie został on jawnie określony przez etap `LoadComplete` (który może zostać odgadnąć, gdy jest to możliwe po etapie `Load`).

Aby to osiągnąć, Zastąp metodę `OnLoadComplete` i wprowadź następujący kod:

[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample6.cs)]

Metoda `OnLoadComplete` zaczyna się od określenia, czy właściwość `Title` nie została jeszcze jawnie ustawiona. Jeśli właściwość `Title` jest `null`, pusty ciąg lub ma wartość "Strona bez tytułu", zostanie ona przypisana do nazwy pliku żądanej strony ASP.NET. Ścieżka fizyczna do żądanej strony ASP.NET `C:\MySites\Tutorial03\Login.aspx`, na przykład, jest dostępna za pośrednictwem właściwości `Request.PhysicalPath`. Metoda `Path.GetFileNameWithoutExtension` służy do ściągania tylko części nazwy pliku, a następnie ta nazwa pliku jest następnie przypisywana do właściwości `Page.Title`.

> [!NOTE]
> Zapraszam do udoskonalenia tej logiki w celu poprawy formatu tytułu. Na przykład jeśli nazwa pliku strony jest `Company-Products.aspx`, powyższy kod spowoduje utworzenie tytułu "produkty firmowe", ale w idealnym przypadku kreska zostanie zastąpiona spacją, tak jak w "produktach firmy". Należy również rozważyć dodanie odstępu w przypadku zmiany wielkości liter. Oznacza to, że należy rozważyć dodanie kodu, który przekształca nazwę pliku `OurBusinessHours.aspx` na tytuł "godzin pracy".

### <a name="having-the-content-pages-inherit-the-base-page-class"></a>Posiadanie stron zawartości dziedziczą klasę strony podstawowej

Teraz należy zaktualizować strony ASP.NET w naszej witrynie, aby dziedziczyć z niestandardowej strony podstawowej (`BasePage`) zamiast klasy `Page`. W tym celu należy przejść do każdej klasy związanej z kodem i zmienić deklarację klasy z:

[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample7.cs)]

Do:

[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample8.cs)]

Po wykonaniu tej czynności odwiedź witrynę za pomocą przeglądarki. Jeśli odwiedzasz stronę, której tytuł jest jawnie ustawiony, na przykład `Default.aspx` lub `About.aspx`, zostanie użyty jawnie określony tytuł. Jeśli jednak zostanie odwiedzana strona, której tytuł nie został zmieniony z domyślnej ("niezatytułowanej strony"), Klasa strony bazowej ustawia tytuł na nazwę pliku strony.

Rysunek 5 przedstawia stronę `MultipleContentPlaceHolders.aspx` wyświetlaną w przeglądarce. Należy zauważyć, że tytuł jest precyzyjnie nazwą pliku strony (mniejszą od rozszerzenia), "MultipleContentPlaceHolders".

[![jeśli tytuł nie zostanie określony jawnie, nazwa pliku strony zostanie automatycznie użyta](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image6.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image5.png)

**Ilustracja 05**. Jeśli tytuł nie jest jawnie określony, nazwa pliku strony jest używana automatycznie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image7.png))

## <a name="step-3-basing-the-page-title-on-the-site-map"></a>Krok 3. oparcie tytułu strony na mapie witryny

ASP.NET oferuje niezawodną strukturę mapy witryny, która umożliwia deweloperom stron Definiowanie hierarchicznej mapy witryny w zewnętrznym zasobie (na przykład pliku XML lub tabeli bazy danych) wraz z kontrolkami sieci Web służącymi do wyświetlania informacji o mapie witryny (takich jak ścieżki mapy witryny). I kontrolki TreeView).

Strukturę mapy witryny można również uzyskać programowo przy użyciu klasy z kodem ASP.NET strony. W ten sposób można automatycznie ustawić tytuł strony na tytuł odpowiedniego węzła w mapie witryny. Zwiększmy klasę `BasePage` utworzoną w kroku 2, aby zapewniała to funkcjonalność. Najpierw musimy utworzyć mapę witryny dla naszej witryny.

> [!NOTE]
> W tym samouczku założono, że czytelnik już zna środowisko ASP. Funkcje mapy witryny sieci. Aby uzyskać więcej informacji na temat korzystania z mapy witryny, zapoznaj się z artykułem moja część serii artykułów, [Badanie ASP. Nawigowanie po witrynie sieci](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx).

### <a name="creating-the-site-map"></a>Tworzenie mapy witryny

System mapy witryny jest zbudowany korzystającego [model dostawcy](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), który oddziela interfejs API mapy witryny od logiki, która deserializacji informacje z mapy witryny między pamięcią a magazynem trwałym. .NET Framework jest dostarczany z [klasą`XmlSiteMapProvider`](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx), która jest domyślnym dostawcą mapy witryny. Jako że jego nazwa oznacza, `XmlSiteMapProvider` używa pliku XML jako magazynu mapy witryny. Użyjmy tego dostawcy do definiowania naszej mapy witryny.

Zacznij od utworzenia pliku mapy witryny w folderze głównym witryny sieci Web o nazwie `Web.sitemap`. Aby to osiągnąć, kliknij prawym przyciskiem myszy nazwę witryny sieci Web w Eksplorator rozwiązań, wybierz polecenie Dodaj nowy element, a następnie wybierz szablon mapa witryny. Upewnij się, że plik ma nazwę `Web.sitemap` i kliknij przycisk Dodaj.

[![dodać pliku o nazwie Web. sitemap do folderu głównego witryny sieci Web](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image9.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image8.png)

**Ilustracja 06**. Dodawanie pliku o nazwie `Web.sitemap` do folderu głównego witryny sieci Web ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image10.png))

Dodaj następujący kod XML do pliku `Web.sitemap`:

[!code-xml[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample9.xml)]

Ten plik XML definiuje hierarchiczną strukturę mapy witryny przedstawioną na rysunku 7.

![Mapa witryny składa się obecnie z trzech węzłów mapy witryny](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image11.png)

**Ilustracja 07**: Mapa witryny jest obecnie złożona z trzech węzłów mapy witryny

Będziemy aktualizować strukturę mapy witryny w przyszłych samouczkach, dodając nowe przykłady.

### <a name="updating-the-master-page-to-include-navigation-web-controls"></a>Aktualizowanie strony głównej w celu uwzględnienia kontrolek sieci Web nawigacji

Teraz, gdy mamy zdefiniowaną mapę witryny, zaktualizuj stronę wzorcową, aby obejmowała kontrolki sieci Web nawigacji. W tym celu Dodajmy kontrolkę ListView do lewej kolumny w sekcji lekcje, która renderuje listę nieuporządkowaną z elementem listy dla każdego węzła zdefiniowanego w mapie witryny.

> [!NOTE]
> Formant ListView jest nowy do ASP.NET w wersji 3,5. Jeśli używasz wcześniejszej wersji programu ASP.NET, zamiast tego użyj kontrolki wzmacniak. Aby uzyskać więcej informacji na temat kontrolki ListView, zobacz [Używanie kontrolek ListView i formantu DataPager w programie ASP.NET 3.5](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx).

Zacznij od usunięcia istniejącego nieuporządkowanego znacznika listy z sekcji lekcji. Następnie przeciągnij kontrolkę ListView z przybornika i upuść ją pod nagłówkiem lekcji. Element ListView znajduje się w sekcji danych przybornika wraz z innymi kontrolkami widoku: GridView, DetailsView i FormView. Ustaw właściwość identyfikator elementu ListView na `LessonsList`.

W Kreatorze konfiguracji źródła danych wybierz opcję powiązania elementu ListView z nową kontrolką SiteMapDataSource o nazwie `LessonsDataSource`. Kontrolka SiteMapDataSource zwraca strukturę hierarchiczną z systemu mapy lokacji.

[![powiązać formant SiteMapDataSource z kontrolką ListView LessonsList](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image13.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image12.png)

**Ilustracja 08**: powiązanie formantu SiteMapDataSource z kontrolką ListView `LessonsList` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image14.png))

Po utworzeniu formantu SiteMapDataSource musimy zdefiniować szablony elementu ListView, aby renderować listę nieuporządkowaną z elementem listy dla każdego węzła zwróconego przez kontrolkę SiteMapDataSource. Można to osiągnąć przy użyciu następującego znacznika szablonu:

[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample10.aspx)]

`LayoutTemplate` generuje znaczniki dla listy nieuporządkowanej (`<ul>...</ul>`), podczas gdy `ItemTemplate` renderuje każdy element zwracany przez SiteMapDataSource jako element listy (`<li>`) zawierający link do konkretnej lekcji.

Po skonfigurowaniu szablonów elementu ListView odwiedź witrynę sieci Web. Jak pokazano na rysunku 9, sekcja lekcji zawiera pojedynczy element punktowany, Strona główna. Gdzie znajdują się lekcje i używanie wielu kontrolek ContentPlaceHolder? SiteMapDataSource jest zaprojektowana w celu zwrócenia hierarchicznego zestawu danych, ale formant ListView może wyświetlać tylko jeden poziom hierarchii. W związku z tym zostanie wyświetlony tylko pierwszy poziom węzłów mapy witryny zwracanych przez SiteMapDataSource.

[![sekcja lekcji zawiera pojedynczy element listy](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image16.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image15.png)

**Ilustracja 09**. sekcja lekcji zawiera pojedynczy element listy ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image17.png))

Aby wyświetlić wiele poziomów, możemy zagnieździć wiele ListViews w `ItemTemplate`. Ta technika została zbadana na [ *stronach wzorcowych i* w samouczku nawigacji witryny](../../data-access/introduction/master-pages-and-site-navigation-cs.md) my [Pracuj z samouczkiem](../../data-access/index.md)dotyczącym danych. Jednak dla tej serii samouczków mapa witryny będzie zawierać tylko dwa poziomy: Strona główna (najwyższego poziomu); i każda lekcja jako element podrzędny. Zamiast poinstruować zagnieżdżony element ListView, możemy SiteMapDataSource, aby nie zwracał węzła początkowego przez ustawienie jego [właściwości`ShowStartingNode`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx) na `false`. Efektem netto jest to, że SiteMapDataSource jest uruchamiany przez zwrócenie drugiej warstwy węzłów mapy lokacji.

Po tej zmianie element ListView wyświetla elementy punktorów dla informacji o i używających wielu formantów ContentPlaceHolder, ale pomija element punktora dla strony głównej. Aby to naprawić, można jawnie dodać element punktora dla strony głównej w `LayoutTemplate`:

[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample11.aspx)]

Konfigurując SiteMapDataSource, aby pominąć węzeł początkowy i jawnie dodać element punktora macierzystego, w sekcji lekcje są teraz wyświetlane zamierzone dane wyjściowe.

[![sekcja lekcji zawiera element punktora dla domu i każdego węzła podrzędnego](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image19.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image18.png)

**Ilustracja 10**. sekcja lekcji zawiera element punktora dla domu i każdego węzła podrzędnego ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image20.png))

### <a name="setting-the-title-based-on-the-site-map"></a>Ustawianie tytułu na podstawie mapy witryny

Korzystając z mapy witryny, możemy zaktualizować nasze klasy `BasePage`, aby używały tytułu określonego na mapie witryny. Podobnie jak w kroku 2, chcemy użyć tytułu węzła mapy witryny, jeśli tytuł strony nie został jawnie ustawiony przez dewelopera strony. Jeśli żądana strona nie ma jawnie ustawionego tytułu strony i nie zostanie znaleziona na mapie witryny, powrócimy do użycia nazwy pliku żądanej strony (mniejszej od rozszerzenia), tak jak w kroku 2. Rysunek 11 przedstawia proces tej decyzji.

![W przypadku braku jawnie ustawionego tytułu strony jest używany tytuł odpowiedniego węzła mapy witryny](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image21.png)

**Rysunek 11**. w przypadku braku jawnie ustawionego tytułu strony jest używany tytuł odpowiedniego węzła mapy witryny

Zaktualizuj metodę `OnLoadComplete` klasy `BasePage` w taki sposób, aby obejmowała następujący kod:

[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample12.cs)]

Tak jak wcześniej Metoda `OnLoadComplete` zaczyna się od określenia, czy tytuł strony został jawnie ustawiony. Jeśli `Page.Title` jest `null`, pusty ciąg lub zostanie przypisana wartość "Strona bez tytułu", kod automatycznie przypisze wartość do `Page.Title`.

Aby określić tytuł, który ma być używany, kod rozpocznie się, odwołując się do [właściwości`CurrentNode`](https://msdn.microsoft.com/library/system.web.sitemap.currentnode.aspx) [klasy`SiteMap`](https://msdn.microsoft.com/library/system.web.sitemap.aspx). `CurrentNode` zwraca wystąpienie [`SiteMapNode`](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) na mapie witryny, które odpowiada aktualnie żądanej stronie. Przy założeniu, że aktualnie żądana strona zostanie znaleziona w ramach mapy witryny, właściwość `Title` `SiteMapNode`zostanie przypisana do tytułu strony. Jeśli aktualnie żądana strona nie znajduje się na mapie witryny, `CurrentNode` zwraca `null` i nazwa pliku żądanej strony jest używana jako tytuł (jak zostało to zrobione w kroku 2).

Na rysunku 12 przedstawiono stronę `MultipleContentPlaceHolders.aspx` wyświetlaną w przeglądarce. Ponieważ tytuł strony nie jest jawnie ustawiony, jego odpowiedni tytuł węzła mapy witryny jest używany.

![Tytuł strony MultipleContentPlaceHolders. aspx jest pobierany z mapy witryny](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image22.png)

**Ilustracja 12**. tytuł strony `MultipleContentPlaceHolders.aspx` jest pobierany z mapy witryny

## <a name="step-4-adding-other-page-specific-markup-to-theheadsection"></a>Krok 4. Dodawanie innego znacznika specyficznego dla strony do sekcji`<head>`

Kroki 1, 2 i 3 zostały przeszukane podczas dostosowywania elementu `<title>` na podstawie strony. Oprócz `<title>`sekcja `<head>` może zawierać elementy `<meta>` i elementy `<link>`. Jak wspomniano wcześniej w tym samouczku, sekcja `<head>` `Site.master`zawiera element `<link>` do `Styles.css`. Ponieważ ten `<link>` element jest zdefiniowany na stronie wzorcowej, znajduje się w sekcji `<head>` dla wszystkich stron zawartości. Jak możemy dowiedzieć się, jak dodać `<meta>` i `<link>` elementy na stronie?

Najprostszym sposobem dodawania zawartości specyficznej dla strony do sekcji `<head>` jest utworzenie kontrolki ContentPlaceHolder na stronie wzorcowej. Mamy już taki element ContentPlaceHolder (o nazwie `head`). W związku z tym, aby dodać niestandardowe znaczniki `<head>`, Utwórz odpowiednią kontrolkę zawartości na stronie i Umieść znacznik w tym miejscu.

Aby zilustrować Dodawanie niestandardowych znaczników `<head>` do strony, dołączymy element `<meta>` Description do naszego bieżącego zestawu stron zawartości. Element `<meta>` Description zawiera krótki opis strony sieci Web; Większość aparatów wyszukiwania zawiera te informacje w postaci pewnego formularza podczas wyświetlania wyników wyszukiwania.

Element `<meta>` Description ma następującą formę:

[!code-html[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample13.html)]

Aby dodać tę adiustację do strony zawartości, Dodaj powyższy tekst do kontrolki zawartość, która jest mapowana do nagłówka elementu ContentPlaceHolder na stronie wzorcowej. Na przykład, aby zdefiniować `<meta>` element Description dla `Default.aspx`, Dodaj następujący znacznik:

[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample14.aspx)]

Ponieważ plik ContentPlaceHolder nie znajduje się w treści strony HTML, znacznik dodany do kontrolki zawartości nie jest wyświetlany w widok Projekt. Aby wyświetlić `<meta>` element opisu, odwiedź `Default.aspx` za pomocą przeglądarki. Po załadowaniu strony Wyświetl źródło i zwróć uwagę, że sekcja `<head>` zawiera znacznik określony w kontrolce zawartość.

Poświęć chwilę na dodanie `<meta>` elementów opisu do `About.aspx`, `MultipleContentPlaceHolders.aspx`i `Login.aspx`.

### <a name="programmatically-adding-markup-to-theheadregion"></a>Programistyczne Dodawanie znaczników do`<head>`regionu

W nagłówku ContentPlaceHolder można deklaratywnie dodać niestandardowe znaczniki do regionu `<head>` strony głównej. Niestandardowe znaczniki mogą być również dodawane programowo. Odwołaj, że właściwość `Header` klasy `Page` zwraca wystąpienie `HtmlHead` zdefiniowane na stronie wzorcowej (`<head runat="server">`).

Możliwość programistycznego dodawania zawartości do regionu `<head>` jest przydatna, gdy zawartość do dodania jest dynamiczna. Być może jest oparta na użytkowniku odwiedzającym stronę; być może jest ono pobierane z bazy danych. Niezależnie od przyczyny można dodać zawartość do `HtmlHead`, dodając formanty do swojej kolekcji formantów, tak więc:

[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample15.cs)]

Powyższy kod dodaje element `<meta>` Keywords do regionu `<head>`, który zawiera rozdzielaną przecinkami listę słów kluczowych opisujących stronę. Należy pamiętać, że w celu dodania `<meta>` znacznika utworzysz wystąpienie [`HtmlMeta`](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlmeta.aspx) , ustaw jego `Name` i `Content` właściwości, a następnie dodaj je do kolekcji `Header``Controls`. Podobnie, aby programowo dodać element `<link>`, Utwórz obiekt [`HtmlLink`](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmllink.aspx) , ustaw jego właściwości, a następnie dodaj go do kolekcji `Controls` `Header`.

> [!NOTE]
> Aby dodać dowolny znacznik, Utwórz wystąpienie [`LiteralControl`](https://msdn.microsoft.com/library/system.web.ui.literalcontrol.aspx) , ustaw jego właściwość `Text`, a następnie dodaj ją do kolekcji `Controls` `Header`.

## <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono różne sposoby dodawania znaczników `<head>`ych do poszczególnych stron. Strona wzorcowa powinna zawierać wystąpienie `HtmlHead` (`<head runat="server">`) z elementem ContentPlaceHolder. Wystąpienie `HtmlHead` umożliwia stronom zawartości programowe uzyskiwanie dostępu do regionu `<head>` i deklaratywne i programowo Ustawianie tytułu strony; formant ContentPlaceHolder umożliwia dodanie znaczników niestandardowych do sekcji `<head>` deklaratywnie przez kontrolkę zawartości.

Szczęśliwe programowanie!

### <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Dynamiczne ustawianie tytułu strony w ASP.NET](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [Badanie ASP. Nawigacja w witrynie sieci](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Jak używać tagów Meta HTML](http://searchenginewatch.com/showPage.html?page=2167931)
- [Strony wzorcowe w ASP.NET](http://www.odetocode.com/articles/419.aspx)
- [Korzystanie z kontrolek ListView i formantu DataPager w programie ASP.NET 3.5](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [Używanie niestandardowej klasy bazowej dla klas ASP.NET stron związanych z kodem](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)

### <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor wielu książek ASP/ASP. NET Books i założyciel of 4GuysFromRolla.com, pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 3,5 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott można uzyskać w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu w [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci liderzy dla tego samouczka to Zack Kowalski i suchi Banerjee. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Poprzednie](multiple-contentplaceholders-and-default-content-cs.md)
> [dalej](urls-in-master-pages-cs.md)

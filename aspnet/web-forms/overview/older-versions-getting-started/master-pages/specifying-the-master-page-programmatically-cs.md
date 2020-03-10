---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-cs
title: Programowe Określanie strony wzorcowejC#() | Microsoft Docs
author: rick-anderson
description: Umożliwia programistyczne Ustawianie strony głównej zawartości za pośrednictwem programu obsługi zdarzeń przed inicjalizacją.
ms.author: riande
ms.date: 07/28/2008
ms.assetid: 7c4a3445-2440-4aee-b9fd-779c05e6abb2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-cs
msc.type: authoredcontent
ms.openlocfilehash: 0db23ea05ba001c2bf9fc5330a60a767caa568a0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78625490"
---
# <a name="specifying-the-master-page-programmatically-c"></a>Programowe określanie strony wzorcowej (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_CS.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_CS.pdf)

> Umożliwia programistyczne Ustawianie strony głównej zawartości za pośrednictwem programu obsługi zdarzeń przed inicjalizacją.

## <a name="introduction"></a>Wprowadzenie

Ponieważ przykład Inaugural w [*tworzeniu układu dla całej witryny za pomocą stron wzorcowych*](creating-a-site-wide-layout-using-master-pages-cs.md), wszystkie strony zawartości odwołują się do ich strony głównej w sposób deklaratywny za pośrednictwem atrybutu `MasterPageFile` w dyrektywie `@Page`. Na przykład następująca dyrektywa `@Page` łączy stronę zawartości ze stroną wzorcową `Site.master`:

[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample1.aspx)]

[Klasa`Page`](https://msdn.microsoft.com/library/system.web.ui.page.aspx) w przestrzeni nazw `System.Web.UI` zawiera [Właściwość`MasterPageFile`](https://msdn.microsoft.com/library/system.web.ui.page.masterpagefile.aspx) , która zwraca ścieżkę do strony wzorcowej strony zawartości; jest to właściwość, która jest ustawiana za pomocą dyrektywy `@Page`. Ta właściwość może również służyć do programistycznego określania strony głównej strony zawartości. Takie podejście jest przydatne, jeśli chcesz dynamicznie przypisać stronę wzorcową w oparciu o czynniki zewnętrzne, takie jak użytkownik odwiedzający stronę.

W tym samouczku dodamy drugą stronę wzorcową do naszej witryny sieci Web i dynamicznie decyduje, która strona wzorcowa ma być używana w środowisku uruchomieniowym.

## <a name="step-1-a-look-at-the-page-lifecycle"></a>Krok 1. sprawdzenie cyklu życia strony

Za każdym razem, gdy żądanie dociera do serwera sieci Web dla strony ASP.NET, która jest stroną zawartości, aparat ASP.NET musi odrzucić kontrolki zawartości strony do odpowiednich kontrolek ContentPlaceHolder na stronie wzorcowej. Ta Fusion tworzy pojedynczą hierarchię formantów, która może następnie przechodzić przez typowy cykl życia strony.

Rysunek 1 ilustruje tę syntezę. Krok 1 na rysunku 1 pokazuje początkową zawartość i hierarchie formantów stron wzorcowych. Na końcu końcowego etapu wstępnego inicjowania kontrolki zawartości na stronie są dodawane do odpowiednich elementów ContentPlaceHolders na stronie wzorcowej (krok 2). Po tej syntezie Strona wzorcowa służy jako katalog główny hierarchii formantów. Ta hierarchia formantów jest następnie dodawana do strony, aby utworzyć końcową hierarchię formantów (krok 3). Wynikiem jest fakt, że hierarchia formantów strony obejmuje hierarchię formantów, która ma odmowę.

[![hierarchie formantów strony wzorcowej i zawartości są odrzucane razem podczas etapu wstępnego inicjowania](specifying-the-master-page-programmatically-cs/_static/image2.png)](specifying-the-master-page-programmatically-cs/_static/image1.png)

**Ilustracja 01**: hierarchie formantów strony wzorcowej i zawartości są odrzucane razem z etapem wstępnego inicjowania ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](specifying-the-master-page-programmatically-cs/_static/image3.png))

## <a name="step-2-setting-themasterpagefileproperty-from-code"></a>Krok 2. Ustawianie właściwości`MasterPageFile`z kodu

Partakes strony wzorcowej w ramach tej syntezy zależy od wartości właściwości `MasterPageFile` obiektu `Page`. Ustawienie atrybutu `MasterPageFile` w dyrektywie `@Page` ma efekt netto przypisywania właściwości `MasterPageFile` `Page`podczas etapu inicjowania, który jest pierwszym etapem cyklu życia strony. Możemy również programowo ustawić tę właściwość. Jednak należy pamiętać, że ta właściwość jest ustawiona przed zastosowaniem fuzji na rysunku 1.

Na początku etapu wstępnego inicjowania obiekt `Page` wywołuje jego [zdarzenie`PreInit`](https://msdn.microsoft.com/library/system.web.ui.page.preinit.aspx) i wywołuje jego [metodę`OnPreInit`](https://msdn.microsoft.com/library/system.web.ui.page.onpreinit.aspx). Aby programowo ustawić stronę wzorcową, można utworzyć procedurę obsługi zdarzeń dla zdarzenia `PreInit` lub zastąpić metodę `OnPreInit`. Przyjrzyjmy się obu podejściem.

Zacznij od otwarcia `Default.aspx.cs`, pliku klasy związanej z kodem dla strony głównej witryny. Dodaj program obsługi zdarzeń dla zdarzenia `PreInit` strony, wpisując następujący kod:

[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample2.cs)]

W tym miejscu możemy ustawić właściwość `MasterPageFile`. Zaktualizuj kod, tak aby przypisywać wartość "~/site.master" do właściwości `MasterPageFile`.

[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample3.cs)]

Jeśli ustawisz punkt przerwania i zaczniesz od debugowania, zobaczysz, że za każdym razem, gdy zostanie wyświetlona strona `Default.aspx` lub gdy na tej stronie znajduje się odświeżenie, zostanie wykonana procedura obsługi zdarzeń `Page_PreInit`, a właściwość `MasterPageFile` zostanie przypisana do "~/site.master".

Alternatywnie można przesłonić metodę `OnPreInit` klasy `Page` i ustawić właściwość `MasterPageFile`. Na potrzeby tego przykładu nie ustawimy strony wzorcowej na określonej stronie, ale zamiast `BasePage`. Odwołaj, że utworzyliśmy niestandardową klasę strony podstawowej (`BasePage`) z powrotem w [*temacie Określanie tytułu, Meta tagów i innych nagłówków HTML na stronie wzorcowej*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) . Obecnie `BasePage` przesłania metodę `OnLoadComplete` klasy `Page`, w której ustawia właściwość `Title` strony na podstawie danych mapy witryny. Zaktualizujmy `BasePage`, aby przesłonić również metodę `OnPreInit`, aby programowo określić stronę wzorcową.

[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample4.cs)]

Ze względu na to, że wszystkie strony zawartości pochodzą z `BasePage`, wszystkie z nich są teraz przypisane do programu programowo. W tym momencie program obsługi zdarzeń `PreInit` w `Default.aspx.cs` jest zbędny; Możesz go usunąć.

### <a name="what-about-thepagedirective"></a>Co z dyrektywą`@Page`?

To, co może być nieco mylące, jest to, że właściwości strony zawartości `MasterPageFile` są teraz określane w dwóch miejscach: programowo w metodzie `OnPreInit` klasy `BasePage`, a także przy użyciu atrybutu `MasterPageFile` w dyrektywie `@Page`ej każdej strony zawartości.

Pierwszy etap cyklu życia strony jest etapem inicjalizacji. Podczas tego etapu Właściwość `MasterPageFile` obiektu `Page` ma przypisaną wartość atrybutu `MasterPageFile` w dyrektywie `@Page` (jeśli jest dostępna). Etap wstępnego inicjowania jest zgodny z etapem inicjalizacji. w tym miejscu można programowo ustawić właściwość `MasterPageFile` obiektu `Page`, zastępując wartość przypisaną z `@Page` dyrektywie. Ponieważ ustawiamy Właściwość `MasterPageFile` obiektu `Page` programowo, można usunąć atrybut `MasterPageFile` z dyrektywy `@Page` bez wpływu na środowisko użytkownika końcowego. Aby przekonać się, jak to zrobić, Usuń atrybut `MasterPageFile` z `@Page` dyrektywy w `Default.aspx` a następnie odwiedź stronę za pośrednictwem przeglądarki. Zgodnie z oczekiwaniami, dane wyjściowe są takie same jak przed usunięciem atrybutu.

Czy właściwość `MasterPageFile` jest ustawiana za pośrednictwem dyrektywy `@Page` lub programowo jest w sposób niewzględny dla środowiska użytkownika końcowego. Jednak atrybut `MasterPageFile` w dyrektywie `@Page` jest używany przez program Visual Studio podczas projektowania, aby utworzyć widok WYSIWYG w projektancie. Jeśli powrócisz do `Default.aspx` w programie Visual Studio i przejdziesz do projektanta, zobaczysz komunikat "błąd strony głównej: Strona zawiera kontrolki wymagające odwołania do strony głównej, ale żadna nie jest określona" (patrz rysunek 2).

Krótko mówiąc, należy pozostawić atrybut `MasterPageFile` w dyrektywie `@Page`, aby uzyskać bogate środowisko projektowania w programie Visual Studio.

[![Visual Studio używa atrybutu MasterPageFile dyrektywy @Page w celu renderowania widoku projektu](specifying-the-master-page-programmatically-cs/_static/image5.png)](specifying-the-master-page-programmatically-cs/_static/image4.png)

**Ilustracja 02**: program Visual Studio używa atrybutu `MasterPageFile` dyrektywy `@Page`, aby renderować widok projektu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](specifying-the-master-page-programmatically-cs/_static/image6.png))

## <a name="step-3-creating-an-alternative-master-page"></a>Krok 3. Tworzenie alternatywnej strony wzorcowej

Ze względu na to, że strona wzorcowa strony zawartości może być ustawiana programowo w czasie wykonywania, możliwe jest dynamiczne ładowanie konkretnej strony wzorcowej na podstawie niektórych zewnętrznych kryteriów. Ta funkcja może być przydatna w sytuacjach, w których układ witryny musi się różnić w zależności od użytkownika. Na przykład aplikacja sieci Web aparatu blogów może umożliwić użytkownikom wybranie układu dla swojego blogu, gdzie każdy układ jest skojarzony z inną stroną wzorcową. W czasie wykonywania, gdy gość przegląda blog użytkownika, aplikacja sieci Web musi określić układ blogu i dynamicznie skojarzyć odpowiednią stronę wzorcową ze stroną zawartości.

Sprawdźmy, jak dynamicznie ładować stronę wzorcową w czasie wykonywania na podstawie niektórych zewnętrznych kryteriów. Nasza witryna sieci Web obecnie zawiera tylko jedną stronę wzorcową (`Site.master`). Potrzebujemy innej strony wzorcowej, aby zilustrować wybór strony wzorcowej w czasie wykonywania. Ten krok koncentruje się na tworzeniu i konfigurowaniu nowej strony wzorcowej. Krok 4 pozwala określić, która strona wzorcowa ma być używana w czasie wykonywania.

Utwórz nową stronę wzorcową w folderze głównym o nazwie `Alternate.master`. Dodaj również nowy arkusz stylów do witryny sieci Web o nazwie `AlternateStyles.css`.

[![dodać kolejną stronę wzorcową i plik CSS do witryny sieci Web](specifying-the-master-page-programmatically-cs/_static/image8.png)](specifying-the-master-page-programmatically-cs/_static/image7.png)

**Ilustracja 03**: Dodawanie kolejnej strony wzorcowej i pliku CSS do witryny sieci Web ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](specifying-the-master-page-programmatically-cs/_static/image9.png))

Zaprojektowano stronę wzorcową `Alternate.master`, aby tytuł był wyświetlany w górnej części strony, wyorodkowany i na granatowym tle. Mam z lewej kolumny i przeniesiono tę zawartość poniżej kontrolki `MainContent` ContentPlaceHolder, która teraz obejmuje całą szerokość strony. Ponadto nixed listę nieuporządkowanych lekcji i zastąpiono ją listą poziomą powyżej `MainContent`. Zaktualizowano także czcionki i kolory używane przez stronę wzorcową (i przez rozszerzenie, strony zawartości). Rysunek 4 przedstawia `Default.aspx` podczas korzystania ze strony wzorcowej `Alternate.master`.

> [!NOTE]
> ASP.NET obejmuje możliwość definiowania *motywów*. Motyw to zbiór obrazów, plików CSS oraz ustawień właściwości kontrolki sieci Web, które można zastosować do strony w czasie wykonywania. Motywy są sposobem na przechodzenie, jeśli układy witryny różnią się tylko w obrazach wyświetlanych i według ich reguł CSS. Jeśli układy różnią się znacznie, na przykład przy użyciu różnych kontrolek sieci Web lub mających radykalnie inny układ, należy użyć oddzielnych stron wzorcowych. Aby uzyskać więcej informacji na temat motywów, zapoznaj się z sekcją dalsze informacje na końcu tego samouczka.

[![naszej stronie zawartości można teraz używać nowego wyglądu i działania](specifying-the-master-page-programmatically-cs/_static/image11.png)](specifying-the-master-page-programmatically-cs/_static/image10.png)

**Rysunek 04**: strony zawartości mogą teraz korzystać z nowego wyglądu i sposobu działania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](specifying-the-master-page-programmatically-cs/_static/image12.png))

Gdy znaczniki strony głównej i zawartości są odrzucane, Klasa `MasterPage` sprawdza, czy każda kontrolka zawartości na stronie zawartości odwołuje się do elementu ContentPlaceHolder na stronie wzorcowej. Wyjątek jest zgłaszany, jeśli znaleziono kontrolkę zawartości odwołującą się do nieistniejącego elementu ContentPlaceHolder. Innymi słowy, jest to konieczne, aby strona wzorcowa przypisana do strony zawartości zawierała element ContentPlaceHolder dla każdej kontrolki zawartości na stronie zawartości.

Strona wzorcowa `Site.master` zawiera cztery kontrolki ContentPlaceHolder:

- `head`
- `MainContent`
- `QuickLoginUI`
- `LeftColumnContent`

Niektóre strony zawartości w naszej witrynie sieci Web zawierają tylko jedną lub dwie kontrolki zawartości; inne osoby zawierają kontrolkę zawartości dla każdego z dostępnych elementów ContentPlaceHolder. Jeśli Twoja nowa strona wzorcowa (`Alternate.master`) może zostać kiedykolwiek przypisana do tych stron zawartości, które mają kontrolki zawartości dla wszystkich elementów ContentPlaceHolder w `Site.master`, jest to niezbędne, aby `Alternate.master` również obejmowały te same kontrolki ContentPlaceHolder jako `Site.master`.

Aby uzyskać `Alternate.master` stronę wzorcową podobną do kopalni (zobacz rysunek 4), Zacznij od zdefiniowania stylów strony wzorcowej w `AlternateStyles.css` arkuszu stylów. Dodaj następujące reguły do `AlternateStyles.css`:

[!code-css[Main](specifying-the-master-page-programmatically-cs/samples/sample5.css)]

Następnie Dodaj następujące deklaratywne znaczniki do `Alternate.master`. Jak widać, `Alternate.master` zawiera cztery kontrolki ContentPlaceHolder z tymi samymi wartościami `ID`, co elementy sterujące elementów ContentPlaceHolder w `Site.master`. Ponadto zawiera kontrolkę ScriptManager, która jest niezbędna dla tych stron w naszej witrynie sieci Web, które używają ASP.NET AJAX Framework.

[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample6.aspx)]

### <a name="testing-the-new-master-page"></a>Testowanie nowej strony wzorcowej

Aby przetestować tę nową stronę wzorcową, zaktualizuj metodę `OnPreInit` klasy `BasePage`, tak aby Właściwość `MasterPageFile` była przypisana wartością "~/Alternate.master", a następnie odwiedź witrynę internetową. Każda Strona powinna działać bez błędów z wyjątkiem dwóch: `~/Admin/AddProduct.aspx` i `~/Admin/Products.aspx`. Dodanie produktu do widoku DetailsView w `~/Admin/AddProduct.aspx` powoduje `NullReferenceException` z wiersza kodu, który próbuje ustawić właściwość `GridMessageText` strony wzorcowej. Podczas odwiedzania `~/Admin/Products.aspx` `InvalidCastException` jest generowany podczas ładowania strony z komunikatem: "nie można rzutować obiektu typu" ASP. alternatywny\_Master "na typ" ASP. site\_Master "."

Te błędy występują, ponieważ Klasa `Site.master` związane z kodem zawiera publiczne zdarzenia, właściwości i metody, które nie są zdefiniowane w `Alternate.master`. Część znaczników tych dwóch stron ma `@MasterType` dyrektywę odwołującą się do `Site.master` strony głównej.

[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample7.aspx)]

Ponadto moduł obsługi zdarzeń `ItemInserted` DetailsView w programie `~/Admin/AddProduct.aspx` zawiera kod, który rzutuje luźno wpisaną Właściwość `Page.Master` na obiekt typu `Site`. Dyrektywa `@MasterType` (w ten sposób używana) oraz Rzutowanie w programie obsługi zdarzeń `ItemInserted` ściśle Couples strony `~/Admin/AddProduct.aspx` i `~/Admin/Products.aspx` na stronę wzorcową `Site.master`.

Aby przerwać ten ścisły sprzęg, możemy `Site.master` i `Alternate.master` pochodzić od wspólnej klasy bazowej, która zawiera definicje dla publicznych członków. Poniżej można zaktualizować dyrektywę `@MasterType`, aby odwoływać się do tego wspólnego typu podstawowego.

### <a name="creating-a-custom-base-master-page-class"></a>Tworzenie niestandardowej klasy podstawowej strony wzorcowej

Dodaj nowy plik klasy do folderu `App_Code` o nazwie `BaseMasterPage.cs` i utwórz go od `System.Web.UI.MasterPage`. Musimy zdefiniować metodę `RefreshRecentProductsGrid` i Właściwość `GridMessageText` w `BaseMasterPage`, ale nie można po prostu przenieść ich z `Site.master`, ponieważ te elementy członkowskie współpracują z kontrolkami sieci Web, które są specyficzne dla `Site.master`ej strony wzorcowej (`RecentProducts` GridView i `GridMessage` etykieta).

Należy skonfigurować `BaseMasterPage` w taki sposób, aby te składowe zostały tam zdefiniowane, ale są faktycznie implementowane przez klasy pochodne `BaseMasterPage`(`Site.master` i `Alternate.master`). Ten typ dziedziczenia jest możliwy poprzez oznaczenie klasy i jej elementów członkowskich jako `abstract`. W skrócie, dodanie słowa kluczowego `abstract` do tych dwóch członków ogłasza, że `BaseMasterPage` nie zaimplementowano `RefreshRecentProductsGrid` i `GridMessageText`, ale te klasy pochodne będą.

Konieczne jest również zdefiniowanie zdarzenia `PricesDoubled` w `BaseMasterPage` i udostępnienie metod przez klasy pochodne w celu podniesienia zdarzenia. Wzorzec używany w .NET Framework do ułatwienia tego zachowania ma na celu utworzenie zdarzenia publicznego w klasie bazowej i dodanie chronionej metody `virtual` o nazwie `OnEventName`. Klasy pochodne mogą następnie wywołać tę metodę w celu wypróbowania zdarzenia lub mogą zastąpić je, aby wykonać kod bezpośrednio przed wystąpieniem zdarzenia lub po nim.

Zaktualizuj swoją klasę `BaseMasterPage` tak, aby zawierała następujący kod:

[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample8.cs)]

Następnie przejdź do klasy `Site.master` związanej z kodem i utwórz ją z `BaseMasterPage`. Ponieważ `BaseMasterPage` jest `abstract` musimy przesłonić te `abstract` członków tutaj w `Site.master`. Dodaj słowo kluczowe `override` do definicji metody i właściwości. Należy również zaktualizować kod, który wywołuje zdarzenie `PricesDoubled` w obsłudze zdarzeń `Click` przycisku `DoublePrice` z wywołaniem metody `OnPricesDoubled` klasy bazowej.

Po wprowadzeniu tych zmian Klasa `Site.master` z kodem powinna zawierać następujący kod:

[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample9.cs)]

Konieczna jest również aktualizacja klasy związanej z kodem `Alternate.master`, która dziedziczy po `BaseMasterPage` i przesłania dwa `abstract` członków. Ale ponieważ `Alternate.master` nie zawiera widoku GridView zawierającego listę najnowszych produktów ani etykiety, która wyświetla komunikat po dodaniu nowego produktu do bazy danych, te metody nie muszą wykonywać żadnych czynności.

[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample10.cs)]

### <a name="referencing-the-base-master-page-class"></a>Odwoływanie się do klasy podstawowej strony wzorcowej

Po ukończeniu klasy `BaseMasterPage` i udostępnieniu przez nas dwóch stron wzorcowych ten ostatni krok polega na aktualizacji `~/Admin/AddProduct.aspx` i `~/Admin/Products.aspx` stron w celu odwoływania się do tego wspólnego typu. Zacznij od zmiany dyrektywy `@MasterType` na obu stronach z:

[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample11.aspx)]

Do:

[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample12.aspx)]

Zamiast odwoływania się do ścieżki pliku Właściwość `@MasterType` teraz odwołuje się do typu podstawowego (`BaseMasterPage`). W związku z tym silnie wpisaną Właściwość `Master` używana w klasach związanych z kodem w obu stronach jest teraz typu `BaseMasterPage` (zamiast typu `Site`). Wraz z tą zmianą w miejscu należy ponownie odwiedzać `~/Admin/Products.aspx`. Wcześniej spowodowało to błąd rzutowania, ponieważ strona jest skonfigurowana do używania strony wzorcowej `Alternate.master`, ale dyrektywa `@MasterType` odwołuje się do pliku `Site.master`. Ale teraz Strona jest renderowana bez błędu. Dzieje się tak, ponieważ strona wzorcowa `Alternate.master` może być rzutowana na obiekt typu `BaseMasterPage` (ponieważ rozszerza go).

Istnieje niewielkie zmiany, które należy wprowadzić w `~/Admin/AddProduct.aspx`. Procedura obsługi zdarzeń `ItemInserted` formantu DetailsView używa właściwości `Master` o jednoznacznie określonym typie i właściwości `Page.Master` o swobodnym typie. Podczas aktualizowania dyrektywy `@MasterType` Naprawiono odwołanie o jednoznacznie określonym typie, ale nadal trzeba zaktualizować odwołanie z określoną swobodną. Zastąp następujący wiersz kodu:

[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample13.cs)]

Poniżej, który rzutuje `Page.Master` na typ podstawowy:

[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample14.cs)]

## <a name="step-4-determining-what-master-page-to-bind-to-the-content-pages"></a>Krok 4. Określanie, którą stronę wzorcową powiązać ze stronami zawartości

Nasza Klasa `BasePage` obecnie ustawia wszystkie właściwości `MasterPageFile` strony zawartości na wartość zakodowaną w fazie wstępnego inicjowania cyklu życia strony. Możemy zaktualizować ten kod, aby oprzeć stronę wzorcową w pewnym zewnętrznym składniku. Być może załadowanie strony wzorcowej zależy od preferencji aktualnie zalogowanego użytkownika. W takim przypadku musimy napisać kod w metodzie `OnPreInit` w `BasePage`, który wyszukuje preferencje strony wzorcowej aktualnie odwiedzanego użytkownika.

Utwórzmy stronę sieci Web, która umożliwia użytkownikowi wybranie używanej strony wzorcowej — `Site.master` lub `Alternate.master` — i Zapisz ten wybór w zmiennej sesji. Zacznij od utworzenia nowej strony sieci Web w katalogu głównym o nazwie `ChooseMasterPage.aspx`. Podczas tworzenia tej strony (lub innych stron zawartości odtąd) nie trzeba powiązać jej ze stroną wzorcową, ponieważ strona wzorcowa jest ustawiana programowo w `BasePage`. Niemniej jednak, jeśli nowa strona nie zostanie powiązana ze stroną wzorcową, domyślne znaczniki deklaratywne nowej strony zawierają formularz sieci Web i inną zawartość dostarczoną przez stronę wzorcową. Musisz ręcznie zastąpić ten znacznik odpowiednimi kontrolkami zawartości. Z tego powodu łatwiej jest utworzyć powiązanie nowej strony ASP.NET z stroną wzorcową.

> [!NOTE]
> Ponieważ `Site.master` i `Alternate.master` mają ten sam zestaw elementów ContentPlaceHolder, nie ma znaczenia, którą stronę wzorcową wybrać podczas tworzenia nowej strony zawartości. W celu zapewnienia spójności zaleca się użycie `Site.master`.

[![dodać nową stronę zawartości do witryny sieci Web](specifying-the-master-page-programmatically-cs/_static/image14.png)](specifying-the-master-page-programmatically-cs/_static/image13.png)

**Ilustracja 05**. Dodawanie nowej strony zawartości do witryny sieci Web ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](specifying-the-master-page-programmatically-cs/_static/image15.png))

Zaktualizuj plik `Web.sitemap`, aby zawierał wpis do tej lekcji. Dodaj następujący znacznik poniżej `<siteMapNode>` dla stron wzorcowych i lekcji ASP.NET AJAX:

[!code-xml[Main](specifying-the-master-page-programmatically-cs/samples/sample15.xml)]

Przed dodaniem jakiejkolwiek zawartości do strony `ChooseMasterPage.aspx` Poświęć chwilę na zaktualizowanie klasy powiązanej z kodem strony, aby dziedziczyć ją po `BasePage` (a nie `System.Web.UI.Page`). Następnie Dodaj kontrolkę DropDownList do strony, ustaw jej Właściwość `ID` na `MasterPageChoice`i Dodaj dwa listy elementów z wartościami `Text` "~/site.master" i "~/Alternate.master".

Dodaj kontrolkę sieci Web przycisku do strony i ustaw jej `ID` i `Text` właściwości na `SaveLayout` i "Zapisz wybór układu". W tym momencie znaczniki deklaratywne strony powinny wyglądać podobnie do następujących:

[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample16.aspx)]

Gdy strona jest najpierw odwiedzona, musi zostać wyświetlona wybrana strona wzorcowa. Utwórz procedurę obsługi zdarzeń `Page_Load` i Dodaj następujący kod:

[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample17.cs)]

Powyższy kod jest wykonywany tylko na pierwszej stronie odwiedzin (a nie na kolejnych ogłaszaniu zwrotnego). Najpierw sprawdza, czy zmienna sesji `MyMasterPage` istnieje. W takim przypadku próbuje znaleźć pasujący element ListItem w `MasterPageChoice` DropDownList. Jeśli zostanie znaleziony pasujący element ListItem, jego właściwość `Selected` jest ustawiona na `true`.

Wymagany jest również kod, który zapisuje wybór użytkownika w zmiennej sesji `MyMasterPage`. Utwórz procedurę obsługi zdarzeń dla zdarzenia `Click` `SaveLayout` przycisku i Dodaj następujący kod:

[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample18.cs)]

> [!NOTE]
> O czas wykonywania procedury obsługi zdarzeń `Click` na ogłaszaniu zwrotnym Strona główna została już wybrana. W związku z tym wybór listy rozwijanej użytkownika nie będzie obowiązywać do momentu odwiedzania następnej strony. `Response.Redirect` Wymusza ponowne żądanie `ChooseMasterPage.aspx`przez przeglądarkę.

Po zakończeniu `ChooseMasterPage.aspx` stronie nasze końcowe zadanie ma mieć `BasePage` przypisać właściwość `MasterPageFile` na podstawie wartości zmiennej sesji `MyMasterPage`. Jeśli zmienna sesji nie jest ustawiona, ma `BasePage` domyślną `Site.master`.

[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample19.cs)]

> [!NOTE]
> Przeniesiono kod, który przypisuje Właściwość `MasterPageFile` obiektu `Page` z programu obsługi zdarzeń `OnPreInit` i do dwóch oddzielnych metod. Ta pierwsza metoda, `SetMasterPageFile`, przypisuje Właściwość `MasterPageFile` do wartości zwracanej przez drugą metodę, `GetMasterPageFileFromSession`. `SetMasterPageFile` metodę `virtual` tak, aby w przyszłości klasy rozszerzające `BasePage` można było opcjonalnie zastąpić je implementacją logiki niestandardowej, jeśli jest to konieczne. Zobaczymy przykład zastąpienia właściwości `SetMasterPageFile` `BasePage`w następnym samouczku.

Korzystając z tego kodu, odwiedź stronę `ChooseMasterPage.aspx`. Początkowo wybrana jest strona wzorcowa `Site.master` (zobacz rysunek 6), ale użytkownik może wybrać inną stronę wzorcową z listy rozwijanej.

[![strony zawartości są wyświetlane przy użyciu strony wzorcowej witryny. Master](specifying-the-master-page-programmatically-cs/_static/image17.png)](specifying-the-master-page-programmatically-cs/_static/image16.png)

**Ilustracja 06**. strony zawartości są wyświetlane przy użyciu strony wzorcowej `Site.master` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](specifying-the-master-page-programmatically-cs/_static/image18.png))

[![strony zawartości są teraz wyświetlane za pomocą alternatywnej strony wzorcowej wzorca](specifying-the-master-page-programmatically-cs/_static/image20.png)](specifying-the-master-page-programmatically-cs/_static/image19.png)

**Ilustracja 07**: strony zawartości są teraz wyświetlane przy użyciu strony wzorcowej `Alternate.master` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](specifying-the-master-page-programmatically-cs/_static/image21.png))

## <a name="summary"></a>Podsumowanie

Po odwiedzeniu strony zawartości, jej kontrolki zawartości są odrzucane przez kontrolki ContentPlaceHolder swojej strony wzorcowej. Strona wzorcowa strony zawartości jest określana przez właściwość `MasterPageFile` klasy `Page`, która jest przypisana do atrybutu `MasterPageFile` dyrektywy `@Page` na etapie inicjowania. Jak pokazano w tym samouczku, można przypisać wartość do właściwości `MasterPageFile`, dopóki to zrobisz przed końcem etapu wstępnego inicjowania. Możliwość programowego określania strony wzorcowej otwiera drzwi dla bardziej zaawansowanych scenariuszy, takich jak dynamiczne wiązanie strony zawartości ze stroną wzorcową w oparciu o czynniki zewnętrzne.

Szczęśliwe programowanie!

### <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Diagram cyklu życia strony ASP.NET](http://emanish.googlepages.com/Asp.Net2.0Lifecycle.PNG)
- [ASP.NET cyklu życia strony](https://msdn.microsoft.com/library/ms178472.aspx)
- [ASP.NET motywy i karnacje — Omówienie](https://msdn.microsoft.com/library/ykzx33wh.aspx)
- [Strony wzorcowe: porady, wskazówki i pułapki](http://www.odetocode.com/articles/450.aspx)
- [Motywy w ASP.NET](http://www.odetocode.com/articles/423.aspx)

### <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor wielu książek ASP/ASP. NET Books i założyciel of 4GuysFromRolla.com, pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 3,5 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott można uzyskać w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu w [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzent potencjalnych klientów dla tego samouczka został suchi Banerjee. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](master-pages-and-asp-net-ajax-cs.md)
> [dalej](nested-master-pages-cs.md)

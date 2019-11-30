---
uid: web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-cs
title: Nazewnictwo identyfikatorów kontrolek na stronachC#zawartości () | Microsoft Docs
author: rick-anderson
description: Ilustruje sposób, w jaki formanty ContentPlaceHolder służy jako kontener nazewnictwa i w związku z tym sprawiają, że programistycznie pracuje z kontrolką (za pośrednictwem FindControl)...
ms.author: riande
ms.date: 06/10/2008
ms.assetid: 1c7d0916-0988-4b4f-9a03-935e4b5af6af
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: e849e5860dc988e112cc3a65d976c16ecdf77416
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74624470"
---
# <a name="control-id-naming-in-content-pages-c"></a>Nazewnictwo identyfikatorów kontrolek na stronach zawartości (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_05_CS.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_05_CS.pdf)

> Ilustruje sposób, w jaki formanty ContentPlaceHolder służy jako kontener nazewnictwa i w związku z tym sprawiają, że programistycznie pracuje z kontrolką (za pośrednictwem FindControl). Analizuje ten problem i obejścia. Omówiono również sposób programowego uzyskiwania dostępu do wyników ClientID.

## <a name="introduction"></a>Wprowadzenie

Wszystkie kontrolki serwera ASP.NET obejmują Właściwość `ID`, która jednoznacznie identyfikuje kontrolkę i jest środkiem, do którego kontrolka jest używana programowo w klasie powiązanej z kodem. Podobnie elementy w dokumencie HTML mogą zawierać atrybut `id`, który jednoznacznie identyfikuje element; te `id` wartości są często używane w skrypcie po stronie klienta do programistycznego odwoływania się do określonego elementu HTML. Z tego względu można założyć, że gdy formant serwera ASP.NET jest renderowany w kodzie HTML, jego wartość `ID` jest używana jako wartość `id` renderowanego elementu HTML. Nie jest to konieczne, ponieważ w pewnych okolicznościach pojedynczy formant z pojedynczą wartością `ID` może występować wiele razy w renderowanej adjustacji. Rozważmy kontrolkę GridView, która zawiera TemplateField z kontrolką kontrolki sieci Web o wartości `ID` ProductName. Gdy widok GridView jest powiązany ze źródłem danych w czasie wykonywania, Ta etykieta jest powtarzana jednokrotnie dla każdego wiersza GridView. Każda renderowana etykieta musi mieć unikatową wartość `id`.

Aby obsłużyć takie scenariusze, ASP.NET umożliwia określenie określonych kontrolek jako kontenerów nazw. Kontener nazewnictwa służy jako nowa przestrzeń nazw `ID`. Wszystkie kontrolki serwera, które znajdują się w kontenerze nazewnictwa, mają renderowane `id` wartość poprzedzoną `ID` kontrolki kontenera nazw. Na przykład klasy `GridView` i `GridViewRow` są kontenerami nazw. W związku z tym kontrolka etykieta zdefiniowana w elemencie GridView TemplateField z `ID` ProductName otrzymuje `id` wartość `GridViewID_GridViewRowID_ProductName`. Ponieważ *GridViewRowID* jest unikatowy dla każdego wiersza GridView, wyniki `id` są unikatowe.

> [!NOTE]
> [Interfejs`INamingContainer`](https://msdn.microsoft.com/library/system.web.ui.inamingcontainer.aspx) służy do wskazywania, że konkretna kontrolka serwera ASP.NET powinna działać jako kontener nazewnictwa. Interfejs `INamingContainer` nie sprawdza żadnych metod, które musi zaimplementować formant serwera; Zamiast tego jest używany jako znacznik. W przypadku generowania renderowanego znacznika, Jeśli kontrolka implementuje ten interfejs, aparat ASP.NET automatycznie prefiksuje wartość `ID` do wartości atrybutów renderowanych `id`. Ten proces został szczegółowo omówiony w kroku 2.

Kontenery nazewnictwa nie tylko zmieniają renderowane `id` wartość atrybutu, ale również mają wpływ na sposób programistyczny przywoływany przez klasę ASP.NET strony z kodem. Metoda `FindControl("controlID")` jest często używana do programistycznego odwoływania się do kontrolki sieci Web. Jednak `FindControl` nie przechodzą przez kontenery nazw. W związku z tym nie można bezpośrednio użyć metody `Page.FindControl`, aby odwoływać się do kontrolek w obrębie elementu GridView lub innego kontenera nazw.

Ponieważ możliwe jest surmised, strony wzorcowe i elementy ContentPlaceHolder są implementowane jako kontenery nazewnictwa. W tym samouczku sprawdzimy, jak strony wzorcowe wpływają na elementy HTML `id` wartości i sposoby programistycznego odwoływania się do kontrolek sieci Web na stronie zawartości przy użyciu `FindControl`.

## <a name="step-1-adding-a-new-aspnet-page"></a>Krok 1. Dodawanie nowej strony ASP.NET

Aby zademonstrować koncepcje omówione w tym samouczku, dodajmy nową stronę ASP.NET do naszej witryny sieci Web. Utwórz nową stronę zawartości o nazwie `IDIssues.aspx` w folderze głównym, a następnie powiąż ją ze stroną wzorcową `Site.master`.

![Dodawanie strony zawartości IDIssues. aspx do folderu głównego](control-id-naming-in-content-pages-cs/_static/image1.png)

**Ilustracja 01**. Dodawanie strony zawartości `IDIssues.aspx` do folderu głównego

Program Visual Studio automatycznie tworzy kontrolkę zawartości dla każdej z czterech elementów ContentPlaceHolder na stronie głównej. Jak zostało to opisane w samouczku dotyczącym [*wielu elementów ContentPlaceHolders i domyślnej zawartości*](multiple-contentplaceholders-and-default-content-cs.md) , Jeśli kontrolka zawartości nie jest obecna, zamiast tego zostanie wyemitowana domyślna zawartość elementu ContentPlaceHolder strony głównej. Ponieważ `QuickLoginUI` i `LeftColumnContent` Elementy ContentPlaceHolders zawierają odpowiedni znacznik domyślny dla tej strony, przejdź dalej i usuń odpowiednie kontrolki zawartości z `IDIssues.aspx`. Na tym etapie deklaracyjne znaczniki strony zawartości powinny wyglądać następująco:

[!code-aspx[Main](control-id-naming-in-content-pages-cs/samples/sample1.aspx)]

W samouczku [*Określanie tytułu, Meta tagów i innych nagłówków HTML na stronie wzorcowej*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) utworzono niestandardową klasę strony podstawowej (`BasePage`), która automatycznie konfiguruje tytuł strony, jeśli nie jest on jawnie ustawiony. Aby strona `IDIssues.aspx` mogła korzystać z tej funkcji, Klasa odnosząca się do kodu strony musi być pochodną klasy `BasePage` (zamiast `System.Web.UI.Page`). Zmodyfikuj definicję klasy związanej z kodem, aby wyglądać następująco:

[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample2.cs)]

Na koniec Zaktualizuj plik `Web.sitemap`, aby zawierał wpis dla tej nowej lekcji. Dodaj element `<siteMapNode>` i ustaw jego atrybuty `title` i `url` na "problemy z nazewnictwem identyfikatorów kontrolek" i `~/IDIssues.aspx`odpowiednio. Po dokonaniu tego dodawania znacznik pliku `Web.sitemap` powinien wyglądać podobnie do poniższego:

[!code-xml[Main](control-id-naming-in-content-pages-cs/samples/sample3.xml)]

Jak pokazano na rysunku 2, nowy wpis mapy witryny w `Web.sitemap` jest natychmiast widoczny w sekcji lekcji w lewej kolumnie.

![Sekcja lekcji zawiera teraz łącze do &quot;problemów z nazewnictwem identyfikatorów sterowania&quot;](control-id-naming-in-content-pages-cs/_static/image2.png)

**Ilustracja 02**. sekcja lekcji zawiera teraz łącze do problemów z nazewnictwem identyfikatorów kontrolek.

## <a name="step-2-examining-the-renderedidchanges"></a>Krok 2: badanie przetworzonych`ID`zmian

Aby lepiej zrozumieć modyfikacje wprowadzone przez aparat ASP.NET do renderowanych wartości `id` formantów serwera, dodajmy kilka kontrolek sieci Web do strony `IDIssues.aspx`, a następnie Wyświetlaj renderowane znaczniki wysyłane do przeglądarki. W tym celu wpisz tekst "Wprowadź swój wiek:", a po nim kontrolkę TextBox w sieci Web. Na stronie Dodaj kontrolkę sieci Web przycisk i kontrolkę etykieta sieci Web. Ustaw właściwości `ID` i `Columns` pola tekstowego na odpowiednio `Age` i 3. Ustaw właściwości `Text` i `ID` przycisku na "Prześlij" i `SubmitButton`. Wyczyść Właściwość `Text` etykiety i ustaw jej `ID` na `Results`.

W tym momencie znaczniki deklaratywne kontrolki zawartości powinny wyglądać podobnie do następujących:

[!code-aspx[Main](control-id-naming-in-content-pages-cs/samples/sample4.aspx)]

Rysunek 3 przedstawia stronę wyświetlaną za pomocą projektanta programu Visual Studio.

[![Strona zawiera trzy kontrolki sieci Web: pole tekstowe, przycisk i etykieta](control-id-naming-in-content-pages-cs/_static/image4.png)](control-id-naming-in-content-pages-cs/_static/image3.png)

**Ilustracja 03**: Strona zawiera trzy kontrolki sieci Web: pole tekstowe, przycisk i etykieta ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](control-id-naming-in-content-pages-cs/_static/image5.png))

Odwiedź stronę za pomocą przeglądarki, a następnie Wyświetl źródło HTML. Jak pokazano poniżej, `id` wartości elementów HTML dla kontrolek tekstowych, przycisków i etykiet sieci Web są kombinacją `ID` wartości formantów sieci Web i `ID` wartości kontenerów nazw na stronie.

[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample5.html)]

Jak wspomniano wcześniej w tym samouczku, zarówno strona wzorcowa, jak i jej elementy ContentPlaceHolder, pełnią rolę kontenerów nazw. W związku z tym oba współtworzą renderowane `ID` wartości formantów zagnieżdżonych. Weź atrybut `id` TextBox, na przykład: `ctl00_MainContent_Age`. Odwołaj, czy `ID` wartość kontrolki TextBox została `Age`. Ta wartość jest poprzedzona wartością `ID` formantu ContentPlaceHolder, `MainContent`. Ponadto ta wartość jest poprzedzona wartością `ID`ną na stronie wzorcowej `ctl00`. Efektem netto jest `id` wartość atrybutu składająca się z `ID` wartości strony głównej, formantu ContentPlaceHolder i samego pola tekstowego.

Na rysunku 4 przedstawiono takie zachowanie. Aby określić renderowany `id` pola tekstowego `Age`, Zacznij od wartości `ID` formantu TextBox, `Age`. Następnie popracuj nad hierarchią formantów. W każdym kontenerze nazewnictwa (te węzły z kolorem brzoskwini), poprzedź bieżącą renderowane `id` przy użyciu `id`kontenera nazw.

![Renderowane atrybuty identyfikatora są oparte na wartościach identyfikatora kontenerów nazw](control-id-naming-in-content-pages-cs/_static/image6.png)

**Ilustracja 04**: renderowane `id` atrybuty są oparte na wartościach `ID` kontenerów nazw

> [!NOTE]
> W miarę omawianej `ctl00` część renderowanego `id` atrybutu stanowi `ID` wartość strony głównej, ale może być zastanawiasz się, jak to `ID` wartość. Nie została ona określona w dowolnym miejscu w naszej głównej lub stronie zawartości. Większość formantów serwera na stronie ASP.NET są dodawane jawnie za pomocą znaczników deklaratywnych strony. Formant `MainContent` ContentPlaceHolder został jawnie określony w znacznikach `Site.master`; `Age` pole tekstowe zostało zdefiniowane `IDIssues.aspx`znaczników. Możemy określić wartości `ID` dla tych typów formantów za pośrednictwem okno Właściwości lub ze składni deklaratywnej. Inne kontrolki, takie jak sama strona wzorcowa, nie są zdefiniowane w znacznikach deklaratywnych. W związku z tym ich wartości `ID` muszą być automatycznie generowane dla nas. Aparat ASP.NET ustawia wartości `ID` w czasie wykonywania dla tych kontrolek, których identyfikatory nie zostały jawnie ustawione. Używa wzorca nazewnictwa `ctlXX`, gdzie *XX* jest sekwencyjnie rosnącej wartości całkowitej.

Ponieważ sama strona wzorcowa służy jako kontener nazewnictwa, kontrolki sieci Web zdefiniowane na stronie wzorcowej mają również zmienione renderowane `id` wartości atrybutów. Na przykład etykieta `DisplayDate` dodana do strony wzorcowej w temacie [*Tworzenie układu dla całej witryny ze stronami wzorcowymi*](creating-a-site-wide-layout-using-master-pages-cs.md) ma następujące renderowane znaczniki:

[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample6.html)]

Należy zauważyć, że `id` atrybut zawiera zarówno wartość `ID` strony głównej (`ctl00`), jak i `ID` wartość kontrolki sieci Web etykiety (`DateDisplay`).

## <a name="step-3-programmatically-referencing-web-controls-viafindcontrol"></a>Krok 3. programowe odwoływanie się do formantów sieci Web za pośrednictwem`FindControl`

Każda kontrolka serwera ASP.NET zawiera metodę `FindControl("controlID")`, która przeszukuje elementy potomne formantu o nazwie *controlID*. Jeśli taki formant zostanie znaleziony, zostanie zwrócony; Jeśli nie zostanie znaleziona zgodna kontrolka, `FindControl` zwraca `null`.

`FindControl` jest przydatne w scenariuszach, w których trzeba uzyskać dostęp do kontrolki, ale nie masz bezpośredniego odwołania do niego. Podczas pracy z kontrolkami sieci Web danych, takimi jak GridView, na przykład kontrolki w polach GridView są definiowane raz w składni deklaracyjnej, ale w czasie wykonywania wystąpienie kontrolki jest tworzone dla każdego wiersza GridView. W związku z tym kontrolki generowane w czasie wykonywania istnieją, ale nie ma bezpośredniego odwołania z klasy powiązanej z kodem. W związku z tym musimy używać `FindControl` do programistycznego działania z określoną kontrolką w polach GridView. (Aby uzyskać więcej informacji na temat używania `FindControl` do uzyskiwania dostępu do formantów w szablonach kontrolki sieci Web danych, zobacz [niestandardowe formatowanie oparte na danych](../../data-access/custom-formatting/custom-formatting-based-upon-data-cs.md).) Ten sam scenariusz występuje, gdy dynamiczne dodawanie kontrolek sieci Web do formularza sieci Web, temat opisany w temacie [Tworzenie interfejsów użytkownika dynamicznego wprowadzania danych](https://msdn.microsoft.com/library/aa479330.aspx).

Aby zilustrować użycie metody `FindControl` w celu wyszukania formantów na stronie zawartości, Utwórz procedurę obsługi zdarzeń dla zdarzenia `Click` `SubmitButton`. W programie obsługi zdarzeń Dodaj następujący kod, który programowo odwołuje się do `Age` TextBox i `Results` etykieta przy użyciu metody `FindControl`, a następnie wyświetla komunikat w `Results` na podstawie danych wejściowych użytkownika.

> [!NOTE]
> Oczywiście nie musimy używać `FindControl` do odwoływania się do kontrolek etykieta i TextBox w tym przykładzie. Możemy odwoływać się do nich bezpośrednio za pośrednictwem ich `ID` wartości właściwości. Używam `FindControl` tutaj, aby zilustrować, co się dzieje w przypadku używania `FindControl` ze strony zawartości.

[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample7.cs)]

Mimo że składnia używana do wywołania metody `FindControl` różni się nieco w pierwszych dwóch wierszach `SubmitButton_Click`, są one semantycznie równoważne. Odwołaj, że wszystkie kontrolki serwera ASP.NET obejmują metodę `FindControl`. Obejmuje to klasę `Page`, z której wszystkie klasy związane z kodem ASP.NET muszą pochodzić od. W związku z tym, wywołanie `FindControl("controlID")` jest równoważne wywołaniu `Page.FindControl("controlID")`, przy założeniu, że metoda `FindControl` nie została zastąpiona w klasie związanej z kodem lub w niestandardowej klasie bazowej.

Po wprowadzeniu tego kodu odwiedź stronę `IDIssues.aspx` za pomocą przeglądarki, wprowadź swój wiek, a następnie kliknij przycisk "Prześlij". Po kliknięciu przycisku "Prześlij" zostanie zgłoszone `NullReferenceException` (patrz rysunek 5).

[![jest wywoływane NullReferenceException](control-id-naming-in-content-pages-cs/_static/image8.png)](control-id-naming-in-content-pages-cs/_static/image7.png)

**Ilustracja 05**: wywołano `NullReferenceException` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](control-id-naming-in-content-pages-cs/_static/image9.png))

W przypadku ustawienia punktu przerwania w programie obsługi zdarzeń `SubmitButton_Click` zobaczysz, że oba wywołania `FindControl` zwracają wartość `null`. `NullReferenceException` jest zgłaszane, gdy spróbujemy uzyskać dostęp do właściwości `Text` pola tekstowego `Age`.

Problem polega na tym, że `Control.FindControl` jedynie elementy potomne *kontrolki*, które znajdują się *w tym samym kontenerze nazewnictwa*. Ze względu na to, że strona wzorcowa stanowi nowy kontener nazewnictwa, wywołanie do `Page.FindControl("controlID")` nigdy nie permeates obiektu strony wzorcowej `ctl00`. (Zapoznaj się z powrotem do rysunku 4, aby wyświetlić hierarchię formantów, która wyświetla obiekt `Page` jako element nadrzędny obiektu strony wzorcowej `ctl00`.) W związku z tym nie znaleziono etykiety `Results` ani pola tekstowego `Age`, a `ResultsLabel` i `AgeTextBox` są przypisane wartości `null`.

Istnieją dwa obejścia tego problemu: możemy przechodzenie do szczegółów, jednego kontenera nazw w danym momencie do odpowiedniej kontrolki; Możemy też utworzyć własną metodę `FindControl`, która permeates nazewnictwa kontenerów. Sprawdźmy każdą z tych opcji.

### <a name="drilling-into-the-appropriate-naming-container"></a>Przechodzenie do odpowiedniego kontenera nazewnictwa

Aby użyć `FindControl` do odwoływania się do etykiety `Results` lub pola tekstowego `Age`, musimy wywołać `FindControl` z kontrolki nadrzędnej w tym samym kontenerze nazewnictwa. Jak pokazano na rysunku 4, formant `MainContent` ContentPlaceHolder jest jedynym elementem nadrzędnym `Results` lub `Age` znajdującym się w tym samym kontenerze nazewnictwa. Innymi słowy wywoływanie metody `FindControl` z kontrolki `MainContent`, jak pokazano w poniższym fragmencie kodu, prawidłowo zwraca odwołanie do kontrolek `Results` lub `Age`.

[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample8.cs)]

Nie możemy jednak korzystać z `MainContent` elementu ContentPlaceHolder z klasy związanej z kodem na stronie zawartości przy użyciu powyższej składni, ponieważ elementy ContentPlaceHolder są zdefiniowane na stronie wzorcowej. Zamiast tego należy użyć `FindControl`, aby uzyskać odwołanie do `MainContent`. Zastąp kod w obsłudze zdarzeń `SubmitButton_Click` następującymi modyfikacjami:

[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample9.cs)]

Jeśli odwiedzasz stronę za pomocą przeglądarki, wprowadź swój wiek, a następnie kliknij przycisk "Prześlij", zostanie zgłoszony `NullReferenceException`. Jeśli ustawisz punkt przerwania w programie obsługi zdarzeń `SubmitButton_Click`, zobaczysz, że ten wyjątek występuje podczas próby wywołania metody `FindControl` obiektu `MainContent`. Obiekt `MainContent` jest `null`, ponieważ metoda `FindControl` nie może zlokalizować obiektu o nazwie "kontrolka mainContent". Przyczyna podstawowa jest taka sama jak w przypadku etykiet `Results` i `Age` pól tekstowych: `FindControl` rozpoczyna wyszukiwanie od góry hierarchii formantów i nie przechodzą na kontenery nazw, ale `MainContent` element ContentPlaceHolder znajduje się na stronie wzorcowej, która jest kontenerem nazewnictwa.

Zanim będziemy mogli używać `FindControl`, aby uzyskać odwołanie do `MainContent`, najpierw potrzebujemy odwołania do kontrolki strony wzorcowej. Gdy mamy odwołanie do strony wzorcowej, możemy uzyskać odwołanie do `MainContent` ContentPlaceHolder za pośrednictwem `FindControl` i, w tym miejscu, odwołań do etykiety `Results` i pola tekstowego `Age` (ponownie przy użyciu `FindControl`). Ale jak uzyskać odwołanie do strony głównej? Sprawdzając, czy atrybuty `id` w renderowanej adjustacji są widoczne, że `ID` wartość strony wzorcowej to `ctl00`. W związku z tym możemy użyć `Page.FindControl("ctl00")`, aby uzyskać odwołanie do strony głównej, a następnie użyć tego obiektu, aby uzyskać odwołanie do `MainContent`i tak dalej. Poniższy fragment kodu ilustruje tę logikę:

[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample10.cs)]

Chociaż ten kod będzie działać, zakłada się, że automatycznie wygenerowana `ID` strony głównej będzie zawsze `ctl00`. Nigdy nie warto tworzyć założeń dotyczących automatycznie generowanych wartości.

Na szczęście odwołanie do strony głównej jest dostępne za pomocą właściwości `Master` klasy `Page`. W związku z tym, zamiast używać `FindControl("ctl00")`, aby uzyskać odwołanie do strony głównej w celu uzyskania dostępu do `MainContent` ContentPlaceHolder, zamiast tego można użyć `Page.Master.FindControl("MainContent")`. Zaktualizuj procedurę obsługi zdarzeń `SubmitButton_Click` przy użyciu następującego kodu:

[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample11.cs)]

Tym razem odwiedzasz stronę za pomocą przeglądarki, przechodząc do swojego wieku, a kliknięcie przycisku "Prześlij" spowoduje wyświetlenie komunikatu na etykiecie `Results` zgodnie z oczekiwaniami.

[![wiek użytkownika jest wyświetlany w etykiecie](control-id-naming-in-content-pages-cs/_static/image11.png)](control-id-naming-in-content-pages-cs/_static/image10.png)

**Ilustracja 06**. wiek użytkownika jest wyświetlany w etykiecie ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](control-id-naming-in-content-pages-cs/_static/image12.png))

### <a name="recursively-searching-through-naming-containers"></a>Cykliczne wyszukiwanie przy użyciu kontenerów nazw

Przykładem tego, że powyższy kod odwołuje się do formantu `MainContent` ContentPlaceHolder ze strony wzorcowej, a następnie `Results` etykieta i `Age` TextBox formantów z `MainContent`, jest ponieważ metoda `Control.FindControl` przeszukuje tylko wewnątrz kontenera nazw *kontrolek*. Posiadanie `FindControl` pozostać w kontenerze nazewnictwa ma sens w większości scenariuszy, ponieważ dwie kontrolki w dwóch różnych kontenerach nazewnictwa mogą mieć te same wartości `ID`. Rozważmy przypadek GridView, który definiuje kontrolkę sieci Web etykieta o nazwie `ProductName` w ramach jednej z używanie TemplateField. Gdy dane są powiązane z elementem GridView w czasie wykonywania, dla każdego wiersza GridView zostanie utworzona etykieta `ProductName`. Jeśli `FindControl` przeszukiwane przez wszystkie kontenery nazewnictwa i wywołane `Page.FindControl("ProductName")`, jakie wystąpienie etykiety ma zwrócić `FindControl`? Etykieta `ProductName` w pierwszym wierszu GridView? W ostatnim wierszu?

Dzięki temu `Control.FindControl` przeszukiwania kontenera nazw *formantów*ma sens w większości przypadków. Istnieją jednak inne przypadki, takie jak te, które pozostały w firmie US, w których mamy unikatowy `ID` we wszystkich kontenerach nazw i chcesz uniknąć lubiane odwołania do każdego kontenera nazw w hierarchii formantów w celu uzyskania dostępu do formantu. Posiadanie `FindControl` Variant, która rekursywnie przeszukuje wszystkie kontenery nazw. Niestety .NET Framework nie obejmuje takiej metody.

Dobra wiadomość polega na tym, że możemy utworzyć własną metodę `FindControl`, która rekursywnie przeszukuje wszystkie kontenery nazw. W rzeczywistości przy użyciu *metod rozszerzających* można wyrównać metodę `FindControlRecursive` do klasy `Control`, aby dołączyć do istniejącej metody `FindControl`.

> [!NOTE]
> Metody rozszerzające to funkcja New do C# 3,0 i Visual Basic 9, które są językami dostarczanymi z .NET Framework wersja 3,5 i Visual Studio 2008. Krótko mówiąc, metody rozszerzające pozwalają deweloperowi utworzyć nową metodę dla istniejącego typu klasy za pomocą specjalnej składni. Aby uzyskać więcej informacji na temat tej przydatnej funkcji, zapoznaj się z artykułem, [rozszerzając podstawowe funkcje typu przy użyciu metod rozszerzających](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx).

Aby utworzyć metodę rozszerzenia, Dodaj nowy plik do folderu `App_Code` o nazwie `PageExtensionMethods.cs`. Dodaj metodę rozszerzenia o nazwie `FindControlRecursive`, która przyjmuje jako wejściowy parametr `string` o nazwie `controlID`. Aby metody rozszerzające działały prawidłowo, należy się upewnić, że sama klasa i jej metody rozszerzające są oznaczone `static`. Ponadto wszystkie metody rozszerzające muszą przyjmować jako ich pierwszy parametr obiekt typu, do którego stosowana jest metoda rozszerzająca, a ten parametr wejściowy musi być poprzedzony słowem kluczowym `this`.

Dodaj następujący kod do pliku klasy `PageExtensionMethods.cs`, aby zdefiniować tę klasę i metodę rozszerzenia `FindControlRecursive`:

[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample12.cs)]

Korzystając z tego kodu, Wróć do klasy powiązanej z kodem `IDIssues.aspx` strony i Skomentuj bieżące wywołania metody `FindControl`. Zastąp je wywołaniami `Page.FindControlRecursive("controlID")`. Nowości dotyczące metod rozszerzania są widoczne bezpośrednio na listach rozwijanych IntelliSense. Jak pokazano na rysunku 7, gdy wpiszesz stronę i okres trafień, Metoda `FindControlRecursive` jest dołączona do listy rozwijanej IntelliSense wraz z innymi metodami klasy `Control`.

[Metody rozszerzenia ![są zawarte w listach rozwijanych IntelliSense](control-id-naming-in-content-pages-cs/_static/image14.png)](control-id-naming-in-content-pages-cs/_static/image13.png)

**Ilustracja 07**. metody rozszerzające są zawarte w listach rozwijanych IntelliSense ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](control-id-naming-in-content-pages-cs/_static/image15.png))

Wprowadź następujący kod do programu obsługi zdarzeń `SubmitButton_Click`, a następnie przetestuj go, odwiedzając stronę, wprowadzając swój wiek, a następnie klikając przycisk "Prześlij". Jak pokazano na rysunku 6, wynikowe wyniki otrzymają komunikat "Jesteś w wieku lat"

[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample13.cs)]

> [!NOTE]
> Ponieważ metody rozszerzające są nowe C# do 3,0 i Visual Basic 9, w przypadku korzystania z programu Visual Studio 2005 nie można używać metod rozszerzających. Zamiast tego należy zaimplementować metodę `FindControlRecursive` w klasie pomocnika. [Rick Strahl](http://www.west-wind.com/WebLog/default.aspx) to przykład w swoim wpisie w blogu, [ASP.NET Maser strony i `FindControl`](http://www.west-wind.com/WebLog/posts/5127.aspx).

## <a name="step-4-using-the-correctidattribute-value-in-client-side-script"></a>Krok 4. Używanie poprawnej wartości atrybutu`id`w skrypcie po stronie klienta

Jak wskazano w tym samouczku, `id` atrybut renderowany przez formant sieci Web jest używany w skrypcie po stronie klienta do programistycznego odwoływania się do określonego elementu HTML. Na przykład poniższy kod JavaScript odwołuje się do elementu HTML za pomocą jego `id` a następnie wyświetla jego wartość w oknie komunikatu modalnego:

[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample14.cs)]

Odwołaj ten element na stronach ASP.NET, które nie zawierają kontenera nazewnictwa, atrybut `id` renderowanego elementu HTML jest identyczny z wartością właściwości `ID` formantu sieci Web. W związku z tym jest to skłonność do twardych kodów w `id` wartości atrybutów do kodu JavaScript. Oznacza to, że jeśli wiesz, że chcesz uzyskać dostęp do kontrolki sieci Web `Age` TextBox za pośrednictwem skryptu po stronie klienta, zrób to za pośrednictwem wywołania `document.getElementById("Age")`.

Problem z tym podejściem polega na tym, że w przypadku używania stron wzorcowych (lub innych formantów kontenera nazw) renderowane `id` HTML nie jest równoznaczne z właściwością `ID` formantu sieci Web. Pierwszym nachyleniem może być odwiedzenie strony za pomocą przeglądarki i wyświetlenie źródła w celu ustalenia rzeczywistego atrybutu `id`. Po poznaniu wartości renderowanej `id` można wkleić ją do wywołania `getElementById`, aby uzyskać dostęp do elementu HTML, z którym należy się połączyć za pomocą skryptu po stronie klienta. Takie podejście jest mniejsze niż idealne, ponieważ pewne zmiany w hierarchii formantów strony lub zmiany właściwości `ID` kontrolek nazewnictwa spowodują zmianę podanego atrybutu `id`, co spowoduje przerwanie kodu JavaScript.

Dobrą wiadomość jest to, że renderowana wartość atrybutu `id` jest dostępna w kodzie po stronie serwera za pomocą [właściwości`ClientID`](https://msdn.microsoft.com/library/system.web.ui.control.clientid.aspx)kontrolki sieci Web. Tej właściwości należy użyć do określenia `id` wartości atrybutu używanej w skrypcie po stronie klienta. Na przykład, aby dodać funkcję JavaScript do strony, która po wywołaniu wyświetla wartość pola tekstowego `Age` w modalnym oknie komunikatu, Dodaj następujący kod do programu obsługi zdarzeń `Page_Load`:

[!code-javascript[Main](control-id-naming-in-content-pages-cs/samples/sample15.js)]

Powyższy kod wprowadza `Age` wartość właściwości ClientID pola tekstowego w wywołaniu JavaScript, aby `getElementById`. Jeśli odwiedzasz Tę stronę za pomocą przeglądarki i zobaczysz źródło HTML, znajdziesz następujący kod JavaScript:

[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample16.html)]

Zwróć uwagę, jak poprawna wartość atrybutu `id`, `ctl00_MainContent_Age`, pojawia się w wywołaniu `getElementById`. Ponieważ ta wartość jest obliczana w czasie wykonywania, działa bez względu na późniejsze zmiany w hierarchii formantów strony.

> [!NOTE]
> Ten przykładowy kod JavaScript pokazuje, jak dodać funkcję języka JavaScript, która poprawnie odwołuje się do elementu HTML renderowanego przez formant serwera. Aby użyć tej funkcji, należy utworzyć dodatkowy kod JavaScript do wywołania funkcji po załadowaniu dokumentu lub gdy określona akcja użytkownika transpires. Aby uzyskać więcej informacji na temat tych i powiązanych tematów, przeczytaj artykuł [Praca z skrypt po stronie klienta](https://msdn.microsoft.com/library/aa479302.aspx).

## <a name="summary"></a>Podsumowanie

Niektóre formanty serwera ASP.NET działają jako kontenery nazewnictwa, które mają wpływ na renderowane `id` wartości atrybutów ich formantów potomnych, a także zakres formantów canvassed przez metodę `FindControl`. W odniesieniu do stron wzorcowych zarówno sama strona wzorcowa, jak i jej elementy sterujące są kontenerami nazw. W związku z tym musimy umieścić nieco więcej pracy w celu programistycznego odwoływania się do kontrolek na stronie zawartości przy użyciu `FindControl`. W tym samouczku zbadamy dwie techniki: przechodzenie do kontrolki ContentPlaceHolder i wywoływanie jej metody `FindControl`; i przetoczmy własną implementację `FindControl`, która rekursywnie przeszukuje wszystkie kontenery nazw.

Oprócz błędów kontenerów nazewnictwa po stronie serwera wprowadza się w odniesieniu do kontrolek sieci Web, występują również problemy po stronie klienta. W przypadku braku kontenerów nazewnictwa wartość właściwości `ID` formantu sieci Web i renderowane `id` wartość atrybutu jest taka sama. Ale przy dodawaniu kontenera nazw renderowany `id` atrybut zawiera zarówno `ID` wartości kontrolki sieci Web, jak i kontenerów nazewnictwa w pochodzenie hierarchii formantów. Te problemy związane z nazewnictwem są nieproblemowe, o ile jest używana Właściwość `ClientID` formantu sieci Web do określenia renderowanej wartości atrybutu `id` w skrypcie po stronie klienta.

Szczęśliwe programowanie!

### <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [ASP.NET strony wzorcowe i `FindControl`](http://www.west-wind.com/WebLog/posts/5127.aspx)
- [Tworzenie dynamicznych interfejsów użytkownika dla wpisów danych](https://msdn.microsoft.com/library/aa479330.aspx)
- [Rozszerzanie podstawowych funkcji typu przy użyciu metod rozszerzających](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx)
- [Instrukcje: odwołanie do zawartości strony wzorcowej ASP.NET](https://msdn.microsoft.com/library/xxwa0ff0.aspx)
- [Strony Mater: porady, wskazówki i pułapki](http://www.odetocode.com/articles/450.aspx)
- [Praca z skryptem po stronie klienta](https://msdn.microsoft.com/library/aa479302.aspx)

### <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor wielu książek ASP/ASP. NET Books i założyciel of 4GuysFromRolla.com, pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 3,5 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott można uzyskać w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu w [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci liderzy dla tego samouczka to Zack Kowalski i suchi Barnerjee. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Poprzednie](urls-in-master-pages-cs.md)
> [dalej](interacting-with-the-master-page-from-the-content-page-cs.md)

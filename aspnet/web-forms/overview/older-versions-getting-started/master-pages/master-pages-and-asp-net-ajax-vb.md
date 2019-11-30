---
uid: web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-vb
title: Strony wzorcowe i ASP.NET AJAX (VB) | Microsoft Docs
author: rick-anderson
description: W tym artykule omówiono opcje używania ASP.NET AJAX i stron wzorcowych. Sprawdza przy użyciu klasy ScriptManagerProxy; w tym artykule omówiono sposób ładowania różnych plików JS dependi...
ms.author: riande
ms.date: 07/11/2008
ms.assetid: 0ee9318c-29bb-4d58-b1dc-94e575b8ae10
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-vb
msc.type: authoredcontent
ms.openlocfilehash: 7be4ff422b91321ff83ed1f1c731c9a0bfe768f1
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74637878"
---
# <a name="master-pages-and-aspnet-ajax-vb"></a>Strony wzorcowe i ASP.NET AJAX (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_08_VB.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_08_VB.pdf)

> W tym artykule omówiono opcje używania ASP.NET AJAX i stron wzorcowych. Sprawdza przy użyciu klasy ScriptManagerProxy; w tym artykule omówiono sposób ładowania różnych plików JS w zależności od tego, czy element ScriptManager jest używany na stronie wzorcowej, czy na stronie zawartości.

## <a name="introduction"></a>Wprowadzenie

W ciągu ostatnich kilku lat więcej deweloperów korzystających z aplikacji sieci Web obsługujących technologię AJAX. Witryna internetowa z obsługą technologii AJAX korzysta z wielu powiązanych technologii sieci Web, aby lepiej reagować na środowisko użytkownika. Tworzenie aplikacji ASP.NET z obsługą technologii AJAX jest bardzo proste dzięki architekturze ASP.NET AJAX firmy Microsoft. ASP.NET AJAX jest wbudowana w ASP.NET 3,5 i Visual Studio 2008; jest on również dostępny jako osobny plik do pobrania dla aplikacji ASP.NET 2,0.

Podczas tworzenia stron sieci Web obsługujących technologię AJAX przy użyciu ASP.NET AJAX Framework należy dodać precyzyjną kontrolkę ScriptManager do każdej i każdej strony korzystającej z platformy. Ponieważ jego nazwa wskazuje, element ScriptManager zarządza skrypt po stronie klienta używany na stronach sieci Web obsługujących technologię AJAX. Jako minimum, obiekt ScriptManager emituje kod HTML, który nakazuje przeglądarce pobranie plików JavaScript, które korzeń bibliotekę kliencką ASP.NET AJAX. Można go również użyć do zarejestrowania niestandardowych plików JavaScript, usług sieci Web obsługujących skrypty i funkcji niestandardowych usług aplikacji.

Jeśli witryna używa stron wzorcowych (zgodnie z oczekiwaniami), nie trzeba dodawać kontrolki ScriptManager do każdej strony pojedynczej zawartości; Zamiast tego można dodać kontrolkę ScriptManager do strony wzorcowej. Ten samouczek pokazuje, jak dodać formant ScriptManager do strony wzorcowej. Przedstawiono w nim również, jak używać formantu ScriptManagerProxy do rejestrowania niestandardowych skryptów i usług skryptów na określonej stronie zawartości.

> [!NOTE]
> Ten samouczek nie eksploruje projektowania ani tworzenia aplikacji sieci Web obsługujących technologię AJAX przy użyciu platformy ASP.NET AJAX. Aby uzyskać więcej informacji na temat korzystania z technologii AJAX, zapoznaj się z filmami wideo i samouczkami ASP.NET AJAX, a także zasobami wymienionymi w sekcji dalsze informacje na końcu tego samouczka.

## <a name="examining-the-markup-emitted-by-the-scriptmanager-control"></a>Badanie znaczników emitowanych przez kontrolkę ScriptManager

Kontrolka ScriptManager emituje znaczniki, które instruują przeglądarkę, aby pobierali pliki JavaScript, które korzeń bibliotekę kliencką ASP.NET AJAX. Dodaje również bit wbudowanego kodu JavaScript do strony, która inicjuje tę bibliotekę. Poniższe znaczniki pokazują zawartość dodaną do renderowanego danych wyjściowych strony zawierającej formant ScriptManager:

[!code-html[Main](master-pages-and-asp-net-ajax-vb/samples/sample1.html)]

Tagi `<script src="url"></script>` instruują przeglądarkę, aby pobierali i wykonywały plik JavaScript pod *adresem URL*. Element ScriptManager emituje trzy takie znaczniki; jeden odwołuje się do pliku `WebResource.axd`, natomiast pozostałe dwa odwołują się do pliku `ScriptResource.axd`. Te pliki nie istnieją w rzeczywistości jako pliki w witrynie sieci Web. Zamiast tego, gdy żądanie jednego z tych plików dociera do serwera sieci Web, aparat ASP.NET analizuje ciąg QueryString i zwraca odpowiednią zawartość JavaScript. Skrypt dostarczany przez te trzy zewnętrzne pliki JavaScript stanowi bibliotekę kliencką ASP.NET AJAX Framework. Inne Tagi `<script>` emitowane przez element ScriptManager obejmują skrypt wbudowany, który inicjuje tę bibliotekę.

Zewnętrzne odwołania do skryptu i skrypt wbudowany emitowane przez ScriptManager są niezbędne dla strony korzystającej z ASP.NET AJAX Framework, ale nie są potrzebne dla stron, które nie używają struktury. W związku z tym może być najlepszym rozwiązaniem, aby dodać element ScriptManager do tych stron, które używają ASP.NET AJAX Framework. Jest to wystarczające, ale jeśli masz wiele stron, które wykorzystują platformę, można dokończyć Dodawanie kontrolki ScriptManager do wszystkich stron — powtarzalne zadanie, aby wypowiedzieć najmniejszy. Alternatywnie można dodać ScriptManager do strony wzorcowej, która następnie wprowadza ten skrypt do wszystkich stron zawartości. Korzystając z tej metody, nie trzeba pamiętać, aby dodać ScriptManager do nowej strony, która używa ASP.NET AJAX Framework, ponieważ jest ona już zawarta na stronie wzorcowej. Krok 1 przeprowadzi Cię przez dodanie elementu ScriptManager do strony głównej.

> [!NOTE]
> Jeśli planujesz włączenie funkcji AJAX w interfejsie użytkownika strony wzorcowej, nie masz wyboru w temacie, musisz dołączyć element ScriptManager na stronie wzorcowej.

Jednym minusemem dodawania elementu ScriptManager do strony głównej jest wyemitowanie powyższego skryptu na *każdej* stronie, niezależnie od tego, czy jest to konieczne. To wyraźnie prowadzi do bezstratnej przepustowości dla tych stron, które mają dołączony element ScriptManager (za pośrednictwem strony głównej), ale nie używają żadnych funkcji ASP.NET AJAX Framework. Ale tylko ilość zajętej przepustowości?

- Rzeczywista zawartość emitowana przez ScriptManager (pokazana powyżej) sumuje nieco od rozmiarze 1 KB.
- Trzy zewnętrzne pliki skryptów, do których odwołuje się element `<script>`, jednak zawierają około 450KB nieskompresowanych danych; w witrynie sieci Web, która korzysta z kompresji gzip, tę łączną przepustowość można zmniejszyć blisko 100KB. Jednak te pliki skryptów są przechowywane w pamięci podręcznej przez przeglądarkę przez jeden rok, co oznacza, że muszą zostać pobrane tylko raz, a następnie mogą być ponownie używane na innych stronach w witrynie.

W najlepszym przypadku, gdy pliki skryptów są buforowane, łączny koszt to rozmiarze 1 KB, co jest znikome. W najgorszym przypadku, jednak gdy pliki skryptów nie zostały jeszcze pobrane, a serwer sieci Web nie używa żadnej formy kompresji — osiągnięta przepustowość jest około 450KB, która może dodawać dowolne miejsce od drugiego lub dwóch połączeń szerokopasmowych do minuty dla  Użytkownicy korzystający z modemów telefonicznych. Dobrą wiadomość polega na tym, że ponieważ zewnętrzne pliki skryptów są buforowane przez przeglądarkę, ten najgorszy scenariusz występuje rzadko.

> [!NOTE]
> Jeśli w dalszym ciągu nie dowiesz się, jak umieścić formant ScriptManager na stronie wzorcowej, weź pod uwagę formularz sieci Web (`<form runat="server">` znacznik na stronie wzorcowej). Każda strona ASP.NET, która używa modelu ogłaszania zwrotnego, musi zawierać dokładnie jeden formularz sieci Web. Dodawanie formularza sieci Web dodaje dodatkową zawartość: kilka ukrytych pól formularzy, `<form>` znacznika i, w razie potrzeby, funkcję JavaScript do inicjowania ogłaszania zwrotnego ze skryptu. Ten znacznik jest niezbędny dla stron, które nie są ogłaszane zwrotnie. To nadmiarowe znaczniki można wyeliminować, usuwając formularz sieci Web ze strony wzorcowej i ręcznie dodając go do każdej strony zawartości, która ją potrzebuje. Jednak korzyści wynikające z posiadania formularza sieci Web na stronie wzorcowej zwiększają wady, które Dodaliśmy niepotrzebnie do niektórych stron zawartości.

## <a name="step-1-adding-a-scriptmanager-control-to-the-master-page"></a>Krok 1. Dodawanie kontrolki ScriptManager do strony wzorcowej

Każda Strona sieci Web, która używa ASP.NET AJAX Framework, musi zawierać dokładnie jedną kontrolkę ScriptManager. Ze względu na to wymaganie, zazwyczaj warto umieścić pojedynczą kontrolkę ScriptManager na stronie wzorcowej, aby wszystkie strony zawartości miały automatycznie dołączoną kontrolkę ScriptManager. Ponadto element ScriptManager musi znajdować się przed dowolnymi kontrolkami serwera ASP.NET AJAX, takimi jak kontrolki UpdatePanel i UpdateProgress. Dlatego najlepiej umieścić element ScriptManager przed dowolnymi kontrolkami ContentPlaceHolder w formularzu sieci Web.

Otwórz stronę wzorcową `Site.master` i Dodaj kontrolkę ScriptManager do strony w formularzu sieci Web, ale przed elementem `<div id="topContent">` (patrz rysunek 1). Jeśli używasz programu Visual Web Developer 2008 lub Visual Studio 2008, formant ScriptManager znajduje się w przyborniku na karcie rozszerzenia AJAX. Jeśli używasz programu Visual Studio 2005, musisz najpierw zainstalować ASP.NET AJAX Framework i dodać kontrolki do przybornika. Odwiedź stronę pobierania ASP.NET AJAX, aby uzyskać strukturę dla ASP.NET 2,0.

Po dodaniu ScriptManager do strony Zmień jej `ID` z `ScriptManager1` na `MyManager`.

[![dodać ScriptManager do strony wzorcowej](master-pages-and-asp-net-ajax-vb/_static/image2.png)](master-pages-and-asp-net-ajax-vb/_static/image1.png)

**Ilustracja 01**. Dodawanie ScriptManager do strony wzorcowej ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-pages-and-asp-net-ajax-vb/_static/image3.png))

## <a name="step-2-using-the-aspnet-ajax-framework-from-a-content-page"></a>Krok 2. Korzystanie z ASP.NET AJAX Framework ze strony zawartości

Po dodaniu kontrolki ScriptManager do strony wzorcowej możemy teraz dodać funkcję ASP.NET AJAX Framework do dowolnej strony zawartości. Utwórzmy nową stronę ASP.NET, która wyświetla losowo wybrany produkt z bazy danych Northwind. Użyjemy kontrolki czasomierza ASP.NET AJAX Framework, aby zaktualizować ten ekran co 15 sekund, pokazując nowy produkt.

Zacznij od utworzenia nowej strony w katalogu głównym o nazwie `ShowRandomProduct.aspx`. Nie zapomnij, aby powiązać tę nową stronę ze stroną wzorcową `Site.master`.

[![dodać nową stronę ASP.NET do witryny sieci Web](master-pages-and-asp-net-ajax-vb/_static/image5.png)](master-pages-and-asp-net-ajax-vb/_static/image4.png)

**Ilustracja 02**. Dodawanie nowej strony ASP.NET do witryny sieci Web ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-pages-and-asp-net-ajax-vb/_static/image6.png))

Odwołaj ten element w temacie Określanie tytułu, tagów Meta i innych nagłówków HTML na stronie wzorcowej [SKM1] — utworzono niestandardową klasę strony podstawowej o nazwie `BasePage`, która wygenerowała tytuł strony, jeśli nie została ustawiona jawnie. Przejdź do klasy powiązanej z kodem `ShowRandomProduct.aspx` strony i utwórz ją od `BasePage` (zamiast z `System.Web.UI.Page`).

Na koniec Zaktualizuj plik `Web.sitemap`, aby zawierał wpis do tej lekcji. Dodaj następujące oznakowanie poniżej `<siteMapNode>` na potrzeby lekcji wzorzec do interakcji ze stroną zawartości:

[!code-xml[Main](master-pages-and-asp-net-ajax-vb/samples/sample2.xml)]

Dodanie tego elementu `<siteMapNode>` jest odzwierciedlone na liście lekcji (patrz rysunek 5).

### <a name="displaying-a-randomly-selected-product"></a>Wyświetlanie losowo wybranego produktu

Wróć do `ShowRandomProduct.aspx`. Z poziomu projektanta przeciągnij formant UpdatePanel z przybornika do kontrolki zawartości `MainContent` i ustaw jej Właściwość `ID` na `ProductPanel`. Element UpdatePanel reprezentuje region na ekranie, który można zaktualizować asynchronicznie za pomocą częściowego ogłaszania zwrotnego strony.

Pierwsze zadanie polega na wyświetleniu informacji o losowo wybranym produkcie w elemencie UpdatePanel. Zacznij od przeciągnięcia kontrolki DetailsView do elementu UpdatePanel. Ustaw właściwość `ID` formantu DetailsView na `ProductInfo` i wyczyść `Height` i `Width` właściwości. Rozwiń tag inteligentny DetailsView i na liście rozwijanej wybierz źródło danych wybierz opcję powiązania widoku DetailsView z nową kontrolką kontrolki SqlDataSource o nazwie `RandomProductDataSource`.

[![powiązać widoku DetailsView z nową kontrolką kontrolki SqlDataSource](master-pages-and-asp-net-ajax-vb/_static/image8.png)](master-pages-and-asp-net-ajax-vb/_static/image7.png)

**Ilustracja 03**. powiązanie widoku DetailsView z nową kontrolką kontrolki SqlDataSource ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-pages-and-asp-net-ajax-vb/_static/image9.png))

Skonfiguruj kontrolkę kontrolki SqlDataSource, aby połączyć się z bazą danych Northwind za pośrednictwem `NorthwindConnectionString` (utworzonego w ramach współpracy ze stroną wzorcową ze strony zawartość [SKM2] — samouczek). Podczas konfigurowania instrukcji SELECT wybierz opcję, aby określić niestandardową instrukcję SQL, a następnie wprowadź następujące zapytanie:

[!code-sql[Main](master-pages-and-asp-net-ajax-vb/samples/sample3.sql)]

Słowo kluczowe `TOP 1` w klauzuli `SELECT` zwraca tylko pierwszy rekord zwrócony przez zapytanie. Funkcja `NEWID()` generuje nową globalnie unikatową wartość identyfikatora (GUID) i może być używana w klauzuli `ORDER BY` do zwracania rekordów tabeli w losowej kolejności.

[![skonfigurować kontrolki SqlDataSource, aby zwracał pojedynczy, losowo wybrany rekord](master-pages-and-asp-net-ajax-vb/_static/image11.png)](master-pages-and-asp-net-ajax-vb/_static/image10.png)

**Rysunek 04**: Konfigurowanie kontrolki SqlDataSource w celu zwrócenia pojedynczego, losowo wybranego rekordu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-asp-net-ajax-vb/_static/image12.png))

Po zakończeniu działania kreatora program Visual Studio tworzy BoundField dla dwóch kolumn zwracanych przez powyższe zapytanie. W tym momencie znaczniki deklaratywne strony powinny wyglądać podobnie do następujących:

[!code-aspx[Main](master-pages-and-asp-net-ajax-vb/samples/sample4.aspx)]

Rysunek 5 przedstawia stronę `ShowRandomProduct.aspx` wyświetlaną w przeglądarce. Kliknij przycisk Odśwież przeglądarki, aby ponownie załadować stronę. należy zobaczyć wartości `ProductName` i `UnitPrice` dla nowego losowo wybranego rekordu.

[![zostanie wyświetlona losowa nazwa i cena produktu](master-pages-and-asp-net-ajax-vb/_static/image14.png)](master-pages-and-asp-net-ajax-vb/_static/image13.png)

**Ilustracja 05**. zostanie wyświetlona losowa nazwa i cena produktu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-pages-and-asp-net-ajax-vb/_static/image15.png))

### <a name="automatically-displaying-a-new-product-every-15-seconds"></a>Automatyczne wyświetlanie nowego produktu co 15 sekund

ASP.NET AJAX Framework zawiera kontrolkę Timer, która wykonuje odświeżenie w określonym czasie. na stronie ogłaszania zwrotnego jest zgłaszane zdarzenie `Tick` czasomierza. Jeśli kontrolka czasomierza jest umieszczana w elemencie UpdatePanel, wyzwala częściowe ogłaszanie zwrotne strony, podczas którego możemy ponownie powiązać dane z DetailsView, aby wyświetlić nowy losowo wybrany produkt.

Aby to osiągnąć, przeciągnij czasomierz z przybornika i upuść go w elemencie UpdatePanel. Zmień `ID` czasomierza z `Timer1` na `ProductTimer` i jej Właściwość `Interval` od 60000 do 15000. Właściwość `Interval` wskazuje liczbę milisekund między ogłaszaniem zwrotnym; ustawienie go na 15000 powoduje, że czasomierz wyzwala częściowe ogłaszanie zwrotne strony co 15 sekund. W tym momencie znaczniki deklaratywne czasomierza powinny wyglądać podobnie do następujących:

[!code-aspx[Main](master-pages-and-asp-net-ajax-vb/samples/sample5.aspx)]

Utwórz procedurę obsługi zdarzeń dla zdarzenia `Tick` czasomierza. W tym obsłudze zdarzeń musimy ponownie powiązać dane z DetailsView, wywołując metodę `DataBind` w widoku DetailsView. Wykonanie tej czynności powoduje, że formant DetailsView ponownie pobierze dane z kontroli źródła danych, co spowoduje wybranie i wyświetlenie nowego losowo wybranego rekordu (podobnie jak podczas ponownego ładowania strony przez kliknięcie przycisku Odśwież przeglądarki).

[!code-vb[Main](master-pages-and-asp-net-ajax-vb/samples/sample6.vb)]

To wszystko. Odwiedź stronę ponownie za pomocą przeglądarki. Początkowo wyświetlane są informacje losowe o produkcie. Jeśli zauważysz, że na ekranie zobaczysz, że po 15 sekundach informacje o nowym produkcie zastępują istniejący ekran.

Aby lepiej zobaczyć, co się dzieje w tym miejscu, dodajmy kontrolkę etykieta do elementu UpdatePanel, który wyświetla czas ostatniej aktualizacji wyświetlania. Dodaj kontrolkę sieci Web etykieta w elemencie UpdatePanel, ustaw jej `ID` na `LastUpdateTime`i wyczyść jej Właściwość `Text`. Następnie Utwórz procedurę obsługi zdarzeń dla zdarzenia `Load` elementu UpdatePanel i Wyświetl bieżącą godzinę w etykiecie. (Zdarzenie `Load` elementu UpdatePanel jest uruchamiane na wszystkich pełnych lub częściowych stronach ogłaszania zwrotnego.)

[!code-vb[Main](master-pages-and-asp-net-ajax-vb/samples/sample7.vb)]

Po wykonaniu tej zmiany Strona zawiera godzinę załadowania aktualnie wyświetlanego produktu. Rysunek 6 przedstawia stronę podczas pierwszej wizyty. Rysunek 7 pokazuje stronę 15 sekund później po "taktie kontrolki czasomierza", a element UpdatePanel został odświeżony w celu wyświetlenia informacji o nowym produkcie.

[![losowo wybrany produkt jest wyświetlany podczas ładowania strony](master-pages-and-asp-net-ajax-vb/_static/image17.png)](master-pages-and-asp-net-ajax-vb/_static/image16.png)

**Ilustracja 06**: losowo wybrany produkt jest wyświetlany podczas ładowania strony ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-pages-and-asp-net-ajax-vb/_static/image18.png))

[![co 15 sekund, zostanie wyświetlony nowy losowo wybrany produkt](master-pages-and-asp-net-ajax-vb/_static/image20.png)](master-pages-and-asp-net-ajax-vb/_static/image19.png)

**Ilustracja 07**: co 15 sekund jest wyświetlany nowy losowo wybrany produkt ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-asp-net-ajax-vb/_static/image21.png))

## <a name="step-3-using-the-scriptmanagerproxy-control"></a>Krok 3. Korzystanie z formantu ScriptManagerProxy

Wraz z uwzględnieniem wymaganego skryptu dla biblioteki klienta ASP.NET AJAX Framework, ScriptManager może również rejestrować niestandardowe pliki JavaScript, odwołania do usług sieci Web obsługujących skrypty oraz niestandardowe uwierzytelnianie, autoryzację i usługi profilowania. Zwykle takie dostosowania są specyficzne dla określonej strony. Jeśli jednak do niestandardowych plików skryptu, odwołań do usługi sieci Web lub uwierzytelniania, autoryzacji lub usług profilów odwołuje się element ScriptManager na stronie wzorcowej, zostaną one dołączone do wszystkich stron w witrynie sieci Web.

Aby dodać dostosowania powiązane z obiektem ScriptManager na zasadzie stronicowania, użyj formantu ScriptManagerProxy. Możesz dodać ScriptManagerProxy do strony zawartości, a następnie zarejestrować niestandardowy plik JavaScript, odwołanie do usługi sieci Web lub uwierzytelnianie, autoryzację lub usługę profilu z ScriptManagerProxy; ma to wpływ na rejestrację tych usług na określonej stronie zawartości.

> [!NOTE]
> Na stronie ASP.NET może znajdować się tylko jedna kontrolka ScriptManager. W związku z tym nie można dodać kontrolki ScriptManager do strony zawartości, Jeśli kontrolka ScriptManager jest już zdefiniowana na stronie wzorcowej. Jedynym celem ScriptManagerProxy jest umożliwienie deweloperom zdefiniowania elementu ScriptManager na stronie wzorcowej, ale nadal istnieje możliwość dodawania dostosowań ScriptManager do poszczególnych stron.

Aby wyświetlić formant ScriptManagerProxy w działaniu, uzupełnimy element UpdatePanel w `ShowRandomProduct.aspx`, aby uwzględnić przycisk, który używa skryptu po stronie klienta do wstrzymania lub wznowienia kontroli czasomierza. Kontrolka Timer ma trzy metody po stronie klienta, których można użyć do osiągnięcia odpowiedniej funkcjonalności:

- `_startTimer()` — uruchamia formant czasomierza
- `_raiseTick()` — powoduje, że kontrolka czasomierza jest równa "Tick", co oznacza ponowne zaksięgowanie i podnoszenie zdarzenia taktu na serwerze
- `_stopTimer()` — wyłącza kontrolkę Timer

Utwórzmy plik JavaScript z zmienną o nazwie `timerEnabled` i funkcją o nazwie `ToggleTimer`. Zmienna `timerEnabled` wskazuje, czy kontrolka czasomierza jest obecnie włączona, czy wyłączona; Domyślna wartość to true. Funkcja `ToggleTimer` akceptuje dwa parametry wejściowe: odwołanie do przycisku pauzy/wznowienia oraz wartość `id` formantu Timer po stronie klienta. Ta funkcja włącza wartość `timerEnabled`, pobiera odwołanie do kontrolki Timer, uruchamia lub zatrzymuje czasomierz (w zależności od wartości `timerEnabled`) i aktualizuje tekst wyświetlany przycisku na "wstrzymanie" lub "wznowienie". Ta funkcja będzie wywoływana za każdym razem, gdy zostanie kliknięty przycisk Wstrzymaj/Wznów.

Zacznij od utworzenia nowego folderu w witrynie sieci Web o nazwie `Scripts`. Następnie Dodaj nowy plik do folderu Scripts o nazwie `TimerScript.js` typu plik JScript.

[![dodać nowego pliku JavaScript do folderu Scripts](master-pages-and-asp-net-ajax-vb/_static/image23.png)](master-pages-and-asp-net-ajax-vb/_static/image22.png)

**Ilustracja 08**: Dodawanie nowego pliku JavaScript do folderu `Scripts` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-pages-and-asp-net-ajax-vb/_static/image24.png))

[![dodano nowy plik JavaScript do witryny sieci Web](master-pages-and-asp-net-ajax-vb/_static/image26.png)](master-pages-and-asp-net-ajax-vb/_static/image25.png)

**Ilustracja 09**. Dodano nowy plik JavaScript do witryny sieci Web ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-pages-and-asp-net-ajax-vb/_static/image27.png))

Następnie Dodaj następujący scrip do pliku `TimerScript.js`:

[!code-csharp[Main](master-pages-and-asp-net-ajax-vb/samples/sample8.cs)]

Teraz będziemy musieli zarejestrować ten niestandardowy plik JavaScript w `ShowRandomProduct.aspx`. Wróć do `ShowRandomProduct.aspx` i Dodaj kontrolkę ScriptManagerProxy do strony; Ustaw `ID`, aby `MyManagerProxy`. Aby zarejestrować niestandardowy plik JavaScript, wybierz kontrolkę ScriptManagerProxy w projektancie, a następnie przejdź do okno Właściwości. Jedna z właściwości jest zatytułowana skrypty. Wybranie tej właściwości powoduje wyświetlenie edytora kolekcji odwołanie do skryptu pokazanego na rysunku 10. Kliknij przycisk Dodaj, aby dodać nowe odwołanie do skryptu, a następnie wprowadź ścieżkę do pliku skryptu we właściwości Path: `~/Scripts/TimerScript.js`.

[![dodać odwołania do skryptu do kontrolki ScriptManagerProxy](master-pages-and-asp-net-ajax-vb/_static/image29.png)](master-pages-and-asp-net-ajax-vb/_static/image28.png)

**Ilustracja 10**. Dodawanie odwołania do skryptu do formantu ScriptManagerProxy ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-pages-and-asp-net-ajax-vb/_static/image30.png))

Po dodaniu odwołania do skryptu, Zaktualizowano znaczniki deklaratywne kontrolki ScriptManagerProxy w celu uwzględnienia kolekcji `<Scripts>` z pojedynczym wpisem `ScriptReference`, jak ilustruje poniższy fragment kodu:

[!code-aspx[Main](master-pages-and-asp-net-ajax-vb/samples/sample9.aspx)]

Wpis `ScriptReference` instruuje ScriptManagerProxy, aby dołączył odwołanie do pliku JavaScript w jego renderowanym znaczniku. Oznacza to, że przez zarejestrowanie skryptu niestandardowego w ScriptManagerProxy wyrenderowane dane wyjściowe na stronie `ShowRandomProduct.aspx` teraz zawiera inny tag `<script src="url"></script>`: `<script src="Scripts/TimerScript.js" type="text/javascript"></script>`.

Teraz możemy wywołać funkcję `ToggleTimer` zdefiniowaną w `TimerScript.js` ze skryptu klienta na stronie `ShowRandomProduct.aspx`. Dodaj następujący kod HTML w elemencie UpdatePanel:

[!code-aspx[Main](master-pages-and-asp-net-ajax-vb/samples/sample10.aspx)]

Spowoduje to wyświetlenie przycisku z tekstem "pauza". Za każdym razem, gdy zostanie kliknięty, funkcja JavaScript `ToggleTimer` jest wywoływana, przekazując odwołanie do przycisku i `id` wartość kontrolki Timer (`ProductTimer`). Zwróć uwagę na składnię uzyskiwania `id` wartości formantu Timer. `<%=ProductTimer.ClientID%>` emituje wartość właściwości `ClientID` kontrolki czasomierza `ProductTimer`. W opisie identyfikatora formantu na stronach zawartości [SKM3] samouczek omawiał różnice między wartością `ID` po stronie serwera a wynikową wartością `id` po stronie klienta i jak `ClientID` zwraca `id`po stronie klienta.

Rysunek 11 przedstawia Tę stronę podczas pierwszej wizyty w przeglądarce. Czasomierz jest obecnie uruchomiony i aktualizuje informacje o wyświetlanych produktach co 15 sekund. Rysunek 12 przedstawia ekran po kliknięciu przycisku wstrzymania. Kliknięcie przycisku wstrzymania powoduje zatrzymanie czasomierza i zaktualizowanie tekstu przycisku na "wznowienie". Informacje o produkcie będą odświeżane (i nadal odświeżane co 15 sekund), gdy użytkownik kliknie przycisk Wznów.

[![kliknij przycisk Wstrzymaj, aby zatrzymać kontrolkę Timer](master-pages-and-asp-net-ajax-vb/_static/image32.png)](master-pages-and-asp-net-ajax-vb/_static/image31.png)

**Ilustracja 11**. kliknij przycisk Wstrzymaj, aby zatrzymać kontrolkę Timer ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-pages-and-asp-net-ajax-vb/_static/image33.png))

[![kliknij przycisk Wznów, aby ponownie uruchomić czasomierz](master-pages-and-asp-net-ajax-vb/_static/image35.png)](master-pages-and-asp-net-ajax-vb/_static/image34.png)

**Ilustracja 12**. kliknij przycisk Wznów, aby ponownie uruchomić czasomierz ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-pages-and-asp-net-ajax-vb/_static/image36.png))

## <a name="summary"></a>Podsumowanie

Podczas kompilowania aplikacji sieci Web obsługujących technologię AJAX przy użyciu ASP.NET AJAX Framework, każda Strona sieci Web obsługująca technologię AJAX zawiera kontrolkę ScriptManager. Aby ułatwić ten proces, można dodać element ScriptManager do strony wzorcowej, zamiast pamiętać o dodaniu elementu ScriptManager do każdej strony zawartości. Krok 1 pokazano, jak dodać element ScriptManager do strony wzorcowej, podczas gdy krok 2 ogląda się w temacie Implementowanie funkcji AJAX na stronie zawartości.

Jeśli zachodzi potrzeba dodania niestandardowych skryptów, odwołań do usług sieci Web z obsługą skryptów lub niestandardowych usług uwierzytelniania, autoryzacji lub profilu do określonej strony zawartości, Dodaj kontrolkę ScriptManagerProxy do strony zawartość, a następnie skonfiguruj w tym miejscu dostosowania. Krok 3 zbadano, jak używać ScriptManagerProxy do rejestrowania niestandardowego pliku JavaScript na określonej stronie zawartości.

Szczęśliwe programowanie!

### <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [ASP.NET — architektura AJAX](../../../../ajax/index.md)
- [Samouczki ASP.NET AJAX](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md)
- [Wideo ASP.NET AJAX](../../../videos/aspnet-ajax/index.md)
- [Kompilowanie interaktywnego interfejsu użytkownika za pomocą Microsoft ASP.NET AJAX](http://aspnet.4guysfromrolla.com/articles/101007-1.aspx)
- [Losowe sortowanie rekordów przy użyciu NEWID](http://www.sqlteam.com/article/using-newid-to-randomly-sort-records)
- [Korzystanie z kontrolki Timer](http://aspnet.4guysfromrolla.com/articles/061808-1.aspx)

### <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor wielu książek ASP/ASP. NET Books i założyciel of 4GuysFromRolla.com, pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 3,5 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott można uzyskać w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu w [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](interacting-with-the-content-page-from-the-master-page-vb.md)
> [dalej](specifying-the-master-page-programmatically-vb.md)

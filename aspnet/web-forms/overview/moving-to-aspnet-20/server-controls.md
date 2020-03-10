---
uid: web-forms/overview/moving-to-aspnet-20/server-controls
title: Formanty serwera | Microsoft Docs
author: microsoft
description: ASP.NET 2,0 rozszerza kontrolę serwera na wiele sposobów. W tym module omówiono niektóre zmiany architektury w sposób ASP.NET 2,0 i Visual Studio 200...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 43f6ac47-76fc-4cf7-8e9f-c18ce673dfd8
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/server-controls
msc.type: authoredcontent
ms.openlocfilehash: c02a633013f061c09141d4f98871848c011a799e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78641443"
---
# <a name="server-controls"></a>Kontrolki serwera

przez [firmę Microsoft](https://github.com/microsoft)

> ASP.NET 2,0 rozszerza kontrolę serwera na wiele sposobów. W tym module zajmiemy się niektórymi zmianami architektury w sposób ASP.NET 2,0 i Visual Studio 2005 z kontrolkami serwera.

ASP.NET 2,0 rozszerza kontrolę serwera na wiele sposobów. W tym module zajmiemy się niektórymi zmianami architektury w sposób ASP.NET 2,0 i Visual Studio 2005 z kontrolkami serwera.

## <a name="view-state"></a>Wyświetl stan

Podstawowa zmiana stanu widoku w ASP.NET 2,0 to znacząca redukcja rozmiaru. Rozważ użycie na stronie tylko kontrolki Calendar. Oto stan widoku w ASP.NET 1,1.

[!code-css[Main](server-controls/samples/sample1.css)]

Teraz jest to stan widoku na identycznej stronie w witrynie ASP.NET 2,0.

[!code-css[Main](server-controls/samples/sample2.css)]

Jest to bardzo znacząca zmiana i biorąc pod uwagę, że stan widoku jest przenoszony i wykonywany za pośrednictwem sieci, ta zmiana może dać deweloperom znaczący wzrost wydajności. Zmniejszenie rozmiaru stanu widoku jest duże ze względu na sposób, w jaki obsługujemy go wewnętrznie. Należy pamiętać, że stan widoku to ciąg szyfrowany algorytmem Base64. Aby lepiej zrozumieć zmianę stanu widoku w ASP.NET 2,0, przyjrzyjmy się zdekodowanym wartościom z powyższych przykładów.

Poniżej znajduje się kod stanu widoku 1,1:

[!code-css[Main](server-controls/samples/sample3.css)]

Może to wyglądać podobnie jak informacje, ale w tym miejscu istnieje wzorzec. W ASP.NET 1. x użyto pojedynczych znaków do identyfikowania typów danych i rozdzielonych wartości przy użyciu znaków &lt;&gt;. "T" w powyższym przykładzie widoku reprezentuje element tryplet. Tryplet zawiera parę niezsynchronizowane listy ArrayLists ("l" reprezentuje ArrayList). Jeden z tych niezsynchronizowane listy ArrayLists zawiera element Int32 ("i") o wartości 1, a drugi zawiera inne tryplet. Tryplet zawiera parę niezsynchronizowane listy ArrayLists itp. Ważne jest, aby pamiętać, że korzystamy z Triplets zawierających pary, możemy identyfikować typy danych za pośrednictwem litery i używać &lt; i &gt; znaków jako ograniczników.

W ASP.NET 2,0, zdekodowany stan widoku wygląda nieco inaczej.

[!code-powershell[Main](server-controls/samples/sample4.ps1)]

Należy zauważyć ogromną zmianę wyglądu dekodowanego stanu widoku. Ta zmiana ma kilka obprzypinania architektury. Stan widoku w ASP.NET 1. x użył LosFormatter do serializacji danych. W 2,0 korzystamy z nowej klasy ObjectStateFormatter. Ta klasa została specjalnie zaprojektowana w celu ułatwienia serializacji i deserializacji stanu widoku i stanu kontroli. (Stan formantu zostanie uwzględniony w następnej sekcji). Istnieje wiele korzyści uzyskanych przez zmianę metody serializacji i deserializacji. Jedną z najbardziej znaczących jest fakt, że w przeciwieństwie do LosFormatter, który używa elementu TextWriter, ObjectStateFormatter używa BinaryWriter. Dzięki temu ASP.NET 2,0 do przechowywania stanu widoku seria bajtów zamiast ciągów. Zrób na przykład liczbę całkowitą. W ASP.NET 1,1, liczba całkowita wymaga 4 bajtów stanu widoku. W ASP.NET 2,0, ta sama liczba całkowita wymaga 1 bajtu. Wprowadzono inne ulepszenia, aby zmniejszyć ilość widoku, który jest przechowywany. Wartości DateTime, na przykład, są teraz przechowywane przy użyciu właściwości "pozostałej" zamiast ciągu.

Tak, jakby wszystkie z nich były niewystarczające, Specjalna uwaga została wypłacona faktem, że jeden z największych odbiorców stanu widoku w 1. x był tym DataGrid i podobną kontrolką. Istotną wadą kontroli, takich jak DataGrid, w której znajduje się stan widoku, jest to, że często zawiera ona duże ilości powtórzonych informacji. W ASP.NET 1. x, te powtórzone informacje były po prostu przechowywane w przedziale i powyżej, co spowoduje wyświetlenie stanu bloated. W ASP.NET 2,0 używamy nowej klasy IndexedString do przechowywania takich danych. Jeśli ciąg powtarza się, po prostu przechowujemy token dla IndexedString i indeks w działającej tabeli obiektów IndexedString.

## <a name="control-state"></a>Stan formantu

Jednym z głównych uchwytów, które deweloperzy mieli ze stanem widoku, był rozmiar dodany do ładunku HTTP. Jak wspomniano wcześniej, jeden z największych odbiorców stanu widoku to formant DataGrid. Aby uniknąć ogromnego stanu widoku wygenerowanego przez element DataGrid, wielu deweloperów po prostu wyłączył stan widoku dla tej kontrolki. Niestety, to rozwiązanie nie było zawsze dobrym rozwiązaniem. Stan widoku w ASP.NET 1. x zawiera nie tylko dane niezbędne do poprawnego działania formantu. Zawiera również informacje dotyczące stanu interfejsu użytkownika kontrolki. Oznacza to, że jeśli chcesz zezwolić na stronicowanie w elemencie DataGrid, musisz włączyć stan widoku nawet wtedy, gdy nie są potrzebne wszystkie informacje o interfejsie użytkownika, które zawierają stan widoku. Jest to scenariusz wszystko-lub-Nothing.

W programie ASP.NET 2,0 stan kontroli rozwiązuje ten problem dobrze przez wprowadzenie stanu sterowania. Stan formantu zawiera dane, które są absolutnie niezbędne do prawidłowego działania formantu. W przeciwieństwie do stanu widoku, nie można wyłączyć stanu formantu. W związku z tym ważne jest, aby dane przechowywane w stanie kontroli były starannie kontrolowane.

> [!NOTE]
> Stan formantu jest utrwalany wraz ze stanem widoku w polu ukryty formularz \_\_stan wyświetlania.

Ten film wideo jest przewodnikiem stanu widoku stanu i kontroli.

![](server-controls/_static/image1.png)

[Otwórz wideo pełnoekranowe](server-controls/_static/state1.wmv)

Aby formant serwera mógł odczytywać i zapisywać informacje o stanie kontroli, należy wykonać trzy czynności.

## <a name="step-1-call-the-registerrequirescontrolstate-method"></a>Krok 1. wywoływanie metody RegisterRequiresControlState

Metoda RegisterRequiresControlState informuje ASP.NET, że kontrolka musi utrwalać stan kontroli. Przyjmuje jeden argument kontrolki typu, który jest zarejestrowanym formantem.

Należy pamiętać, że rejestracja nie jest zachowywana na żądanie żądania. W związku z tym ta metoda musi być wywoływana dla każdego żądania, jeśli formant ma zachować stan kontroli. Zaleca się, aby metoda została wywołana w funkcji OnInit.

[!code-csharp[Main](server-controls/samples/sample5.cs)]

## <a name="step-2-override-savecontrolstate"></a>Krok 2. zastępowanie SaveControlState

Metoda SaveControlState zapisuje zmiany stanu formantu od momentu ostatniego wyksięgowania z powrotem. Zwraca obiekt reprezentujący stan kontrolki.

## <a name="step-3-override-loadcontrolstate"></a>Krok 3. zastępowanie LoadControlState

Metoda LoadControlState ładuje zapisany stan do kontrolki. Metoda przyjmuje jeden argument typu Object, który przechowuje zapisany stan dla kontrolki.

## <a name="full-xhtml-compliance"></a>Pełna zgodność XHTML

Każdy deweloper sieci Web wie o znaczeniu standardów w aplikacjach sieci Web. Aby można było zarządzać środowiskiem programistycznym opartym na standardach, ASP.NET 2,0 jest w pełni zgodny ze standardem XHTML. W związku z tym wszystkie Tagi są renderowane zgodnie ze standardami XHTML w przeglądarkach, które obsługują język HTML 4,0 lub nowszy.

Definicja DOCTYPE w ASP.NET 1,1 była następująca:

[!code-html[Main](server-controls/samples/sample6.html)]

W ASP.NET 2,0 domyślna definicja DOCTYPE jest następująca:

[!code-html[Main](server-controls/samples/sample7.html)]

W przypadku wybrania tej opcji można zmienić domyślną zgodność XHTML za pośrednictwem węzła xhtmlConformance w pliku konfiguracji. Na przykład następujący węzeł w pliku Web. config zmieni zgodność XHTML na XHTML 1,0 Strict:

[!code-xml[Main](server-controls/samples/sample8.xml)]

W przypadku wybrania tej opcji można również skonfigurować ASP.NET tak, aby korzystała z starszej konfiguracji używanej w ASP.NET 1. x w następujący sposób:

[!code-xml[Main](server-controls/samples/sample9.xml)]

## <a name="adaptive-rendering-using-adapters"></a>Renderowanie adaptacyjne przy użyciu kart

W ASP.NET 1. x plik konfiguracyjny zawiera sekcję &lt;browserCaps&gt;, która wypełnił obiekt HttpBrowserCapabilities. Ten obiekt umożliwia deweloperowi określenie, jakie urządzenie wprowadza konkretne żądanie i odpowiednio renderuje kod. W ASP.NET 2,0 Model został ulepszony i teraz używa nowej klasy ControlAdapter. Klasa ControlAdapter zastępuje zdarzenia w cyklu życia kontrolki i kontroluje renderowanie formantów na podstawie możliwości agenta użytkownika. Możliwości określonego agenta użytkownika są definiowane przy użyciu pliku definicji przeglądarki (pliku z rozszerzeniem przeglądarki) przechowywanego w c:\Windows\Microsoft.NET\Framework\v2.0.\*\*\*\*folderze \CONFIG\Browsers.

> [!NOTE]
> Klasa ControlAdapter jest klasą abstrakcyjną.

Podobnie jak w przypadku &lt;browserCaps&gt; sekcji 1. x plik definicji przeglądarki używa wyrażenia regularnego do analizowania ciągu agenta użytkownika w celu zidentyfikowania przeglądarki żądającej. Definiuje ona określone możliwości dla tego agenta użytkownika. ControlAdapter renderuje kontrolkę za pomocą metody Render. W związku z tym w przypadku zastąpienia metody renderowania nie należy wywoływać renderowania w klasie bazowej. Wykonanie tej operacji może spowodować, że renderowanie występuje dwa razy, raz dla karty i jeden raz dla samego formantu.

## <a name="developing-a-custom-adapter"></a>Tworzenie adaptera niestandardowego

Możesz opracować własną kartę niestandardową, dziedziczących z ControlAdapter. Ponadto można dziedziczyć z klasy abstrakcyjnej PageAdapter w przypadkach, gdy karta jest wymagana dla strony. Mapowanie formantów do karty niestandardowej jest realizowane za pośrednictwem &lt;controlAdapters&gt; elementu w pliku definicji przeglądarki. Na przykład poniższy kod XML z pliku definicji przeglądarki mapuje formant menu na klasę MenuAdapter:

[!code-html[Main](server-controls/samples/sample10.html)]

Korzystanie z tego modelu ułatwia deweloperom kontroli przekierowanie do określonego urządzenia lub przeglądarki. Deweloper może również mieć pełną kontrolę nad sposobem renderowania stron na każdym urządzeniu.

## <a name="per-device-rendering"></a>Renderowanie na urządzenie

Właściwości kontrolek serwera w ASP.NET 2,0 można określić dla poszczególnych urządzeń przy użyciu prefiksu specyficznego dla przeglądarki. Na przykład poniższy kod zmieni tekst etykiety w zależności od tego, które urządzenie jest używane do przeglądania strony.

[!code-aspx[Main](server-controls/samples/sample11.aspx)]

Gdy strona zawierająca tę etykietę zostanie wyświetlona z przeglądarki Internet Explorer, etykieta wyświetli tekst mówiący "przeglądasz program Internet Explorer". Po przejściu strony z przeglądarki Firefox etykieta wyświetli tekst "przeglądasz z przeglądarki Firefox". Gdy strona zostanie przeszukana z dowolnego innego urządzenia, zostanie wyświetlona wartość "przeglądasz z nieznanego urządzenia". Każdą właściwość można określić przy użyciu tej specjalnej składni.

## <a name="setting-focus"></a>Ustawianie fokusu

ASP.NET 1. x deweloperzy często pytają, jak ustawić początkowy fokus dla konkretnej kontrolki. Na przykład na stronie logowania przydatne jest, aby pole tekstowe identyfikatora użytkownika miało fokus podczas pierwszego ładowania strony. W ASP.NET 1. x wykonaj te czynności, pisząc jakiś skrypt po stronie klienta. Mimo że taki skrypt to zadanie proste, nie jest już konieczne w ASP.NET 2,0, dzięki czemu Metoda SetFocus. Metoda SetFocus przyjmuje jeden argument wskazujący, że formant powinien otrzymać fokus. Ten argument może być IDENTYFIKATORem klienta kontrolki jako ciąg lub nazwą formantu serwera jako obiektem sterowania. Na przykład, aby ustawić początkowy fokus na kontrolkę TextBox o nazwie txtUserID podczas pierwszego ładowania strony, Dodaj następujący kod do strony\_obciążenie:

[!code-csharp[Main](server-controls/samples/sample12.cs)]

--lub

[!code-csharp[Main](server-controls/samples/sample13.cs)]

ASP.NET 2,0 używa procedury obsługi WebResource. axd (omówionej wcześniej) do renderowania funkcji po stronie klienta, która ustawia fokus. Nazwa funkcji po stronie klienta to WebForm,\_autofocus, jak pokazano poniżej:

[!code-html[Main](server-controls/samples/sample14.html)]

Alternatywnie można użyć metody focus dla kontrolki, aby ustawić początkowy fokus dla tej kontrolki. Metoda Focus dziedziczy z klasy Control i jest dostępna dla wszystkich kontrolek ASP.NET 2,0. Możliwe jest również ustawienie fokusu na konkretną kontrolę w przypadku wystąpienia błędu walidacji. Zostanie ono omówione w późniejszym module.

## <a name="new-server-controls-in-aspnet-20"></a>Nowe kontrolki serwera w ASP.NET 2,0

Poniżej przedstawiono nowe kontrolki serwera w programie ASP.NET 2,0. W późniejszych modułach przejdziemy do bardziej szczegółowych informacji na temat niektórych z nich.

## <a name="imagemap-control"></a>ImageMap — formant

Kontrolka ImageMap umożliwia dodanie punktów aktywności do obrazu, który może inicjować ogłoszenie zwrotne lub przejść do adresu URL. Dostępne są trzy typy hotspotów; CircleHotSpot, RectangleHotSpot i PolygonHotSpot. Hotspoty są dodawane za pośrednictwem edytora kolekcji w programie Visual Studio lub programowo w kodzie. Brak dostępnego interfejsu użytkownika do rysowania hotspotów na obrazie. Współrzędne i rozmiar lub promień punktu aktywnego muszą być określone deklaratywnie. Nie istnieje również wizualna reprezentacja elementu hotspot w projektancie. Jeśli hotspot jest skonfigurowany pod kątem nawigowania do adresu URL, adres URL jest określany za pośrednictwem właściwości NavigateUrl elementu hotspot. W przypadku hotspotu z powrotem Właściwość PostBackValue umożliwia przekazywanie ciągu w ogłoszeniu zwrotnym, który można pobrać w kodzie po stronie serwera.

![Edytor kolekcji HotSpotów w programie Visual Studio](server-controls/_static/image1.jpg)

**Rysunek 1**: Edytor kolekcji hotspotów w programie Visual Studio

## <a name="bulletedlist-control"></a>BulletedList — formant

Formant BulletedList jest listą punktowaną, która może łatwo być powiązana z danymi. Lista może być uporządkowana (numerowana) lub nieuporządkowana za pośrednictwem właściwości bullet. Każdy element na liście jest reprezentowany przez obiekt ListItem.

![Kontrolka BulletedList w programie Visual Studio](server-controls/_static/image1.gif)

**Rysunek 2**. kontrolka BulletedList w programie Visual Studio

## <a name="hiddenfield-control"></a>HiddenField — formant

Kontrolka HiddenField dodaje ukryte pole formularza do strony, wartość, która jest dostępna w kodzie po stronie serwera. Wartość pola ukrytego formularza powinna być zwykle niezmieniona między wpisami z powrotem. Jednak złośliwy użytkownik może zmienić wartość przed opublikowaniem. W takim przypadku formant HiddenField zgłosi zdarzenie ValueChanged. Jeśli masz poufne informacje w kontrolce HiddenField i chcesz się upewnić, że pozostanie niezmienione, należy obsłużyć zdarzenie ValueChanged w kodzie.

## <a name="fileupload-control"></a>FileUpload — formant

Kontrolka FileUpload w ASP.NET 2,0 umożliwia przekazywanie plików na serwer sieci Web za pośrednictwem strony ASP.NET. Ta kontrolka jest bardzo podobna do klasy ASP.NET 1. x HtmlInputFile z kilkoma wyjątkami. W ASP.NET 1. x zaleca się, aby Właściwość PostedFile była sprawdzana pod kątem wartości null, aby określić, czy masz dobry plik. Kontrolka FileUpload w ASP.NET 2,0 dodaje nową właściwość HasFile, której można użyć do tego samego celu i jest nieco bardziej wydajna.

Właściwość PostedFile jest nadal dostępna dla dostępu do obiektu HttpPostedFile, ale niektóre funkcje HttpPostedFile są teraz dostępne wewnętrznie z kontrolką FileUpload. Na przykład, aby zapisać przekazany plik w ASP.NET 1. x, należy wywołać metodę SaveAs dla obiektu HttpPostedFile. Przy użyciu formantu FileUpload w ASP.NET 2,0, należy wywołać metodę SaveAs na samej kontrolce FileUpload.

Inna znacząca zmiana w zachowaniu 2,0 (i prawdopodobnie najbardziej znacząca zmiana) polega na tym, że nie jest już konieczne załadowanie całego przekazanego pliku do pamięci przed jego zapisaniem. W 1. x każdy przekazany plik jest zapisywany całkowicie do pamięci przed zapisaniem na dysku. Ta architektura uniemożliwia przekazywanie dużych plików.

W ASP.NET 2,0 atrybut requestLengthDiskThreshold elementu httpRuntime umożliwia skonfigurowanie, ile kilobajtów jest przechowywanych w buforze w pamięci przed zapisaniem na dysku.

**Ważne**: dokumentacja MSDN (i dokumentacja w innym miejscu) określa, że ta wartość jest wyrażona w bajtach (nie kilobajtach) i że wartością domyślną jest 256. Wartość jest w rzeczywistości określona w kilobajtach, a wartość domyślna to 80. Mając wartość domyślną 80K, firma Microsoft gwarantuje, że bufor nie kończy się na stertie dużego obiektu.

## <a name="wizard-control"></a>Kontrolka Kreatora

Dość często spotyka się, aby napotkać ASP.NET deweloperzy zoptymalizowaniem przy próbie zebrania informacji w serii "stron" przy użyciu paneli lub transferu z strony do strony. Częściej niż nie, Endeavor jest frustrujące i jest czasochłonna. Nowa kontrolka kreatora rozwiązuje problemy, umożliwiając stosowanie liniowych i nieliniowych kroków w interfejsie Kreatora, z którym użytkownicy znają. Kontrolka kreatora przedstawia formularze wejściowe w szeregu kroków. Każdy krok jest określonego typu określonego przez właściwość StepType kontrolki. Dostępne są następujące typy kroków:

| **Typ kroku** | **Wyjaśnienie** |
| --- | --- |
| Auto | Kreator automatycznie określa typ kroku w zależności od jego pozycji w hierarchii kroków. |
| Początek | Pierwszy krok, często używany do prezentowania instrukcji wprowadzającej. |
| Krok | Normalny krok. |
| Zakończ | Ostatni krok, zazwyczaj używany do prezentowania przycisku, aby zakończyć pracę kreatora. |
| Pełne | Przedstawia komunikat informujący o powodzeniu lub niepowodzeniu. |

> [!NOTE]
> Kontrolka kreatora śledzi swój stan przy użyciu stanu kontrolki ASP.NET. W związku z tym Właściwość EnableViewState można ustawić na wartość false bez żadnego szkody.

Ten film wideo to przewodnik po kontrolce kreatora.

![](server-controls/_static/image2.png)

[Otwórz wideo pełnoekranowe](server-controls/_static/wizard1.wmv)

## <a name="localize-control"></a>Lokalizowanie formantu

Zlokalizowany formant jest podobny do kontrolki literału. Jednak formant lokalizuje ma właściwość **mode** , która kontroluje sposób, w jaki jest renderowany znacznik dodawany do niego. Właściwość Mode obsługuje następujące wartości:

| **Wyst** | **Wyjaśnienie** |
| --- | --- |
| Przekształcanie | Znaczniki są przekształcane zgodnie z protokołem przeglądarki wysyłającej żądanie. |
| Kazują | Znaczniki są renderowane jako-is. |
| Kodowanie | Znaczniki dodawane do formantu są kodowane przy użyciu narzędzia HtmlEncode. |

## <a name="multiview-and-view-controls"></a>Widoku wielu elementów i kontrolki widoku

Kontrolka widoku wielu elementów działa jako kontener dla kontrolek widok, a kontrolka widoku działa jako kontener (podobnie jak kontrolka panel) dla innych kontrolek. Każdy widok w kontrolce widoku wielu elementów jest reprezentowany przez pojedynczy formant widoku. Pierwszy formant widoku w widoku wielu elementów to 0, drugi jest widokiem 1 itd. Możesz przełączyć widoki, określając ActiveViewIndex kontrolki widoku wielu elementów.

## <a name="substitution-control"></a>Formant podstawiania

Formant podstawienia jest używany w połączeniu z pamięcią podręczną ASP.NET. W przypadkach, w których chcesz korzystać z buforowania, ale masz fragmenty strony, które należy zaktualizować dla każdego żądania (innymi słowy, fragmenty strony, które są wykluczone z buforowania), składnik podstawiania udostępnia doskonałe rozwiązanie. Kontrolka nie renderuje w rzeczywistości żadnych danych wyjściowych. Zamiast tego jest on powiązany z metodą w kodzie po stronie serwera. Gdy żądaną stroną, wywoływana jest metoda, a zwracana Adiustacja jest renderowana zamiast formantu podstawiania.

Metoda, z którą jest powiązana kontrolka podstawienia, jest określana za pomocą właściwości **MethodName** . Ta metoda musi spełniać następujące kryteria:

- Musi być to metoda statyczna (współdzielona w języku VB).
- Akceptuje jeden parametr typu HttpContext.
- Zwraca ciąg reprezentujący znacznik, który powinien zastąpić formant na stronie.

Kontrolka podstawienia nie ma możliwości modyfikacji żadnej innej kontrolki na stronie, ale ma dostęp do bieżącego obiektu HttpContext za pośrednictwem jego parametru.

## <a name="gridview-control"></a>Formant GridView

Formant GridView jest zamiennikiem formantu DataGrid. Ten formant zostanie szczegółowo omówiony w późniejszym module.

## <a name="detailsview-control"></a>Formant DetailsView

Kontrolka DetailsView umożliwia wyświetlenie pojedynczego rekordu ze źródła danych oraz jego edytowanie lub usuwanie. Jest ono omówione bardziej szczegółowo w późniejszym module.

## <a name="formview-control"></a>Kontrolka FormView

Formant FormView służy do wyświetlania pojedynczego rekordu ze źródła danych w konfigurowalnym interfejsie. Jest ono omówione bardziej szczegółowo w późniejszym module.

## <a name="accessdatasource-control"></a>AccessDataSource — formant

Formant AccessDataSource służy do tworzenia powiązań danych z bazą danych programu Access. Jest ono omówione bardziej szczegółowo w późniejszym module.

## <a name="objectdatasource-control"></a>ObjectDataSource — formant

Kontrolka ObjectDataSource służy do obsługi architektury trójwarstwowej, dzięki czemu formanty mogą być powiązane z danymi do obiektu biznesowego warstwy środkowej, a nie z modelem dwuwarstwowym, w którym formanty są powiązane bezpośrednio ze źródłem danych. Zostanie ona omówiona bardziej szczegółowo w późniejszym module.

## <a name="xmldatasource-control"></a>Formant XmlDataSource

Formant XmlDataSource jest używany do tworzenia powiązań danych ze źródłem danych XML. Jest ono omówione bardziej szczegółowo w późniejszym module.

## <a name="sitemapdatasource-control"></a>SiteMapDataSource — Formant

Kontrolka SiteMapDataSource zapewnia powiązanie danych dla kontrolek nawigacyjnych lokacji w oparciu o mapę witryny. Zostanie ona omówiona bardziej szczegółowo w późniejszym module.

## <a name="sitemappath-control"></a>SiteMapPath Control

Kontrolka ścieżki mapy witryny wyświetla szereg linków nawigacyjnych, które są często określane jako nawigacyjne. Jest ono omówione bardziej szczegółowo w późniejszym module.

## <a name="menu-control"></a>Kontrolka menu

Kontrolka menu wyświetla dynamiczne menu przy użyciu języka DHTML. Jest ono omówione bardziej szczegółowo w późniejszym module.

## <a name="treeview-control"></a>TreeView — Formant

Formant TreeView służy do wyświetlania hierarchicznego widoku drzewa danych. Jest ono omówione bardziej szczegółowo w późniejszym module.

## <a name="login-control"></a>Kontrola logowania

Kontrolka logowania zapewnia mechanizm logowania do witryny sieci Web. Jest ono omówione bardziej szczegółowo w późniejszym module.

## <a name="loginview-control"></a>Widoku logowania — formant

Formant widoku logowania umożliwia wyświetlanie różnych szablonów na podstawie stanu logowania użytkownika. Jest ono omówione bardziej szczegółowo w późniejszym module.

## <a name="passwordrecovery-control"></a>PasswordRecovery — formant

Kontrolka PasswordRecovery służy do pobierania zapomnianych haseł przez użytkowników aplikacji ASP.NET. Jest ono omówione bardziej szczegółowo w późniejszym module.

## <a name="loginstatus"></a>Stanu logowania

Kontrolka stanu logowania wyświetla stan logowania użytkownika. Jest ono omówione bardziej szczegółowo w późniejszym module.

## <a name="loginname"></a>LoginName

Kontrolka LoginName wyświetla nazwę użytkownika po zalogowaniu się do aplikacji ASP.NET. Jest ono omówione bardziej szczegółowo w późniejszym module.

## <a name="createuserwizard"></a>CreateUserWizard

Formancie CreateUserWizard to konfigurowalny Kreator, który umożliwia użytkownikom tworzenie konta członkowstwa ASP.NET do użycia w aplikacji ASP.NET. Jest ono omówione bardziej szczegółowo w późniejszym module.

## <a name="changepassword"></a>ChangePassword

Kontrolka ChangePassword umożliwia użytkownikom zmianę hasła dla aplikacji ASP.NET. Jest ono omówione bardziej szczegółowo w późniejszym module.

## <a name="various-webparts"></a>Różne składniki Web Part

ASP.NET 2,0 jest dostarczany z różnymi składniki Web Part. Zostaną one szczegółowo omówione w późniejszym module.

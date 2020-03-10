---
uid: web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
title: Model strony ASP.NET 2,0 | Microsoft Docs
author: microsoft
description: W ASP.NET 1. x deweloperzy mieli wybór między wbudowanym modelem kodu a modelem kodu związanym z kodem. Za pomocą atrybutu src można zaimplementować kod w tle...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: af4575a3-0ae3-4638-ba4d-218fad7a1642
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
msc.type: authoredcontent
ms.openlocfilehash: bcb71b2b5a484e8756406867e08e8aa699a9024d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78547650"
---
# <a name="the-aspnet-20-page-model"></a>Model strony ASP.NET 2,0

przez [firmę Microsoft](https://github.com/microsoft)

> W ASP.NET 1. x deweloperzy mieli wybór między wbudowanym modelem kodu a modelem kodu związanym z kodem. Za pomocą atrybutu src lub atrybutu CodeBehind dyrektywy @Page można zaimplementować kod w tle. W ASP.NET 2,0 deweloperzy nadal mają wybór między kodem wbudowanym i kodem związanym, ale wprowadzono znaczne ulepszenia modelu związanego z kodem.

W ASP.NET 1. x deweloperzy mieli wybór między wbudowanym modelem kodu a modelem kodu związanym z kodem. Za pomocą atrybutu src lub atrybutu CodeBehind dyrektywy @Page można zaimplementować kod w tle. W ASP.NET 2,0 deweloperzy nadal mają wybór między kodem wbudowanym i kodem związanym, ale wprowadzono znaczne ulepszenia modelu związanego z kodem.

## <a name="improvements-in-the-code-behind-model"></a>Udoskonalenia w modelu związanym z kodem

Aby w pełni zrozumieć zmiany w modelu związanym z kodem w ASP.NET 2,0, najlepszym rozwiązaniem jest szybkie sprawdzenie modelu w postaci ASP.NET 1. x.

## <a name="the-code-behind-model-in-aspnet-1x"></a>Model związany z kodem w ASP.NET 1. x

W ASP.NET 1. x model związany z kodem obejmuje plik ASPX (formularz WebForm) i plik związany z kodem zawierający kod programistyczny. Te dwa pliki zostały połączone przy użyciu dyrektywy @Page w pliku ASPX. Każda kontrolka na stronie ASPX miała odpowiadającą deklarację w pliku związanym z kodem jako zmienną wystąpienia. Plik związany z kodem zawiera również kod dla powiązania zdarzenia i wygenerowany kod wymagany przez projektanta programu Visual Studio. Ten model działał dość dobrze, ale ponieważ każdy element ASP.NET na stronie ASPX wymaga odpowiedniego kodu w pliku związanym z kodem, nie było żadnego prawdziwego rozdzielenia kodu i zawartości. Jeśli na przykład Projektant dodał nową kontrolkę serwera do pliku ASPX poza IDE programu Visual Studio, aplikacja spowodowałaby przerwanie ze względu na brak deklaracji tej kontrolki w pliku związanym z kodem.

## <a name="the-code-behind-model-in-aspnet-20"></a>Model związany z kodem w ASP.NET 2,0

ASP.NET 2,0 znacznie usprawnia ten model. W ASP.NET 2,0 kod jest zaimplementowany przy użyciu nowych *klas częściowych* , które znajdują się w ASP.NET 2,0. Klasa odnosząca się do kodu w ASP.NET 2,0 jest zdefiniowana jako Klasa częściowa, co oznacza, że zawiera tylko część definicji klasy. Pozostała część definicji klasy jest generowana dynamicznie przez ASP.NET 2,0 przy użyciu strony ASPX w środowisku uruchomieniowym lub podczas prekompilowania witryny sieci Web. Łącze między plikiem związanym z kodem a stroną ASPX nadal jest ustanawiane za pomocą dyrektywy @ Page. Jednak zamiast atrybutu CodeBehind lub src, ASP.NET 2,0 używa teraz atrybutu atrybutu 'codefile. Atrybut Inherits służy również do określania nazwy klasy dla strony.

Typowa Dyrektywa @ Page może wyglądać następująco:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample1.aspx)]

Typowa definicja klasy w pliku z kodem ASP.NET 2,0 może wyglądać następująco:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample2.cs)]

> [!NOTE]
> C#i Visual Basic są jedynymi językami zarządzanymi, które obecnie obsługują klasy częściowe. W związku z tym deweloperzy korzystający z języka J# nie będą mogli używać modelu związanego z kodem w ASP.NET 2,0.

Nowy model ulepsza model związany z kodem, ponieważ deweloperzy będą mieli teraz pliki kodu, które zawierają tylko kod utworzony przez siebie. Zapewnia również prawdziwą separację kodu i zawartości, ponieważ w pliku związanym z kodem nie ma deklaracji zmiennej wystąpień.

> [!NOTE]
> Ze względu na to, że Klasa częściowa strony ASPX ma miejsce powiązania zdarzenia, Visual Basic deweloperzy mogą uzyskać niewielki wzrost wydajności dzięki użyciu słowa kluczowego Handles w kodzie, aby powiązać zdarzenia. C#nie ma żadnego równoważnego słowa kluczowego.

## <a name="new--page-directive-attributes"></a>Nowe atrybuty dyrektywy @ Page

ASP.NET 2,0 dodaje wiele nowych atrybutów do dyrektywy @ Page. Następujące atrybuty są nowe w ASP.NET 2,0.

## <a name="async"></a>Async

Atrybut Async umożliwia skonfigurowanie strony do wykonania asynchronicznie. Dobrze pokryć strony asynchroniczne w dalszej części tego modułu.

## <a name="asynctimeout"></a>AsyncTimeout

Określono limit czasu dla stron asynchronicznych. Wartość domyślna to 45 sekund.

## <a name="codefile"></a>CodeFile

Atrybut atrybutu 'codefile jest zamiennikiem atrybutu CodeBehind w programie Visual Studio 2002/2003.

### <a name="codefilebaseclass"></a>CodeFileBaseClass

Atrybut CodeFileBaseClass jest używany w przypadkach, gdy wiele stron ma pochodzić z pojedynczej klasy bazowej. Ze względu na implementację częściowych klas w ASP.NET bez tego atrybutu Klasa bazowa, która używa udostępnionych pól wspólnych, do kontroli odwołań zadeklarowanych na stronie ASPX nie będzie działała prawidłowo, ponieważ usługa ASP. W związku z tym, jeśli chcesz, aby wspólna Klasa bazowa dla dwóch lub więcej stron w ASP.NET, musisz zdefiniować klasę bazową w atrybucie CodeFileBaseClass, a następnie pochodnie każdej klasy stron z tej klasy bazowej. Atrybut atrybutu 'codefile jest również wymagany, gdy ten atrybut jest używany.

## <a name="compilationmode"></a>Kompilacjamode

Ten atrybut umożliwia ustawienie właściwości Kompilacjamode strony ASPX. Właściwość CompilationMode jest wyliczeniem zawierającym wartości **zawsze** **, autoi** **nigdy**. Wartość domyślna to **zawsze**. Ustawienie **automatycznie** uniemożliwi ASP.NETom dynamiczne Kompilowanie strony, jeśli jest to możliwe. Wykluczenie stron z kompilacji dynamicznej zwiększa wydajność. Jednakże jeśli wykluczona Strona zawiera kod, który musi być skompilowany, podczas przeglądania strony zostanie wygenerowany błąd.

## <a name="enableeventvalidation"></a>EnableEventValidation

Ten atrybut określa, czy odświeżenie i zdarzenia wywołania zwrotnego są weryfikowane. Gdy ta opcja jest włączona, argumenty do ogłaszania zwrotnego lub wywołania zwrotnym są sprawdzane w celu upewnienia się, że pochodzą one z formantu serwera, który był pierwotnie renderowany.

## <a name="enabletheming"></a>EnableTheming

Ten atrybut określa, czy motywy ASP.NET są używane na stronie. Wartość domyślna to **false**. Motywy ASP.NET są omówione w [module 10](profiles-themes-and-web-parts.md).

## <a name="linepragmas"></a>LinePragmas

Ten atrybut określa, czy podczas kompilacji należy dodać dyrektywy pragma wiersza. Dyrektywy pragma wiersza są opcjami używanymi przez debugery do oznaczania określonych sekcji kodu.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostback

Ten atrybut określa, czy kod JavaScript jest wprowadzany do strony w celu utrzymania położenia przewijania między ogłaszaniem zwrotnym. Domyślnie ten atrybut ma **wartość false** .

Gdy ten atrybut ma **wartość true**, ASP.NET &lt;dodaje blok&gt; skryptu do ogłaszania zwrotnego, który będzie wyglądać następująco:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample3.html)]

Należy pamiętać, że src dla tego bloku skryptu to WebResource. axd. Ten zasób nie jest ścieżką fizyczną. Po zażądaniu tego skryptu ASP.NET dynamicznie kompiluje skrypt.

### <a name="masterpagefile"></a>MasterPageFile

Ten atrybut określa główny plik strony dla bieżącej strony. Ścieżka może być względna lub bezwzględna. Strony wzorcowe zostały omówione w [module 4](master-pages.md).

## <a name="stylesheettheme"></a>StyleSheetTheme

Ten atrybut umożliwia przesłonięcie właściwości wyglądu interfejsu użytkownika zdefiniowanego przez motyw ASP.NET 2,0. Motywy zostały omówione w [module 10](profiles-themes-and-web-parts.md).

## <a name="theme"></a>Tematów

Określa motyw strony. Jeśli wartość nie zostanie określona dla atrybutu StyleSheetTheme, atrybut Theme zastępuje wszystkie style zastosowane do kontrolek na stronie.

## <a name="title"></a>Tytuł

Ustawia tytuł strony. Określona tutaj wartość zostanie wyświetlona w &lt;tytule&gt; elementu renderowanej strony.

### <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

Ustawia wartość wyliczenia ViewStateEncryptionMode. Dostępne wartości to **zawsze** **, autoi** **nigdy**. Wartość domyślna to **Auto.** Gdy ten atrybut jest ustawiony na wartość **Auto, stan**wyświetlania jest szyfrowany jest żądaniem sterowania przez wywołanie metody **RegisterRequiresViewStateEncryption** .

## <a name="setting-public-property-values-via-the--page-directive"></a>Ustawianie publicznych wartości właściwości za pomocą dyrektywy @ Page

Kolejną nową możliwością dyrektywy @ Page w ASP.NET 2,0 jest możliwość ustawienia początkowej wartości właściwości publicznych klasy bazowej. Załóżmy na przykład, że masz publiczną właściwość o nazwie **SomeText** w klasie bazowej i d, która ma zostać zainicjowana do **Hello** podczas ładowania strony. Można to zrobić, ustawiając wartość w dyrektywie @ page, np.:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample4.aspx)]

Atrybut **SomeText** dyrektywy @ Page ustawia początkową wartość właściwości SomeText w klasie bazowej na *Hello!* . Poniższy film przedstawia przewodnik konfigurowania wartości początkowej właściwości publicznej w klasie bazowej przy użyciu dyrektywy @ Page.

![](the-asp-net-2-0-page-model/_static/image1.png)

[Otwórz wideo pełnoekranowe](the-asp-net-2-0-page-model/_static/setprop1.wmv)

## <a name="new-public-properties-of-the-page-class"></a>Nowe właściwości publiczne klasy Page

Następujące właściwości publiczne są nowe w programie ASP.NET 2,0.

## <a name="apprelativetemplatesourcedirectory"></a>AppRelativeTemplateSourceDirectory

Zwraca ścieżkę względną aplikacji do strony lub kontrolki. Na przykład dla strony znajdującej się w http://app/folder/page.aspxWłaściwość zwraca wartość ~/folder/.

## <a name="apprelativevirtualpath"></a>AppRelativeVirtualPath

Zwraca względną ścieżkę katalogu wirtualnego do strony lub kontrolki. Na przykład dla strony znajdującej się w http://app/folder/page.aspxWłaściwość zwraca wartość ~/folder/Page.aspx.

## <a name="asynctimeout"></a>AsyncTimeout

Pobiera lub ustawia limit czasu używany na potrzeby obsługi stron asynchronicznych. (Strony asynchroniczne zostaną omówione w dalszej części tego modułu).

## <a name="clientquerystring"></a>ClientQueryString

Właściwość tylko do odczytu, która zwraca część ciągu zapytania żądanego adresu URL. Ta wartość jest zakodowana w adresie URL. Aby zdekodować, można użyć metody UrlDecode klasy HttpServerUtility.

## <a name="clientscript"></a>ClientScript

Ta właściwość zwraca obiekt ClientScriptManager, który może być używany do zarządzania ASP. sieci emisji skryptu po stronie klienta. (Klasa ClientScriptManager jest omówiona w dalszej części tego modułu).

## <a name="enableeventvalidation"></a>EnableEventValidation

Ta właściwość określa, czy sprawdzanie poprawności zdarzeń jest włączone dla zdarzeń odświeżania i wywołania zwrotnego. Po włączeniu argumentów do ogłaszania zwrotnego lub wywołania zwrotnym są weryfikowane, aby upewnić się, że pochodzą one z kontrolki serwera, która była pierwotnie renderowana.

## <a name="enabletheming"></a>EnableTheming

Ta właściwość pobiera lub ustawia wartość logiczną określającą, czy motyw ASP.NET 2,0 ma zastosowanie do strony.

## <a name="form"></a>Formularz

Ta właściwość zwraca formularz HTML na stronie ASPX jako obiekt parametr HtmlForm.

## <a name="header"></a>Nagłówek

Ta właściwość zwraca odwołanie do obiektu HtmlHead, który zawiera nagłówek strony. Zwrócony obiekt HtmlHead można użyć do pobierania/ustawiania arkuszy stylów, tagów Meta itp.

## <a name="idseparator"></a>IdSeparator

Ta właściwość tylko do odczytu pobiera znak, który jest używany do oddzielania identyfikatorów kontroli, gdy ASP.NET kompiluje unikatowy identyfikator dla kontrolek na stronie. Metoda nie jest przeznaczona do bezpośredniego użycia z poziomu kodu.

## <a name="isasync"></a>IsAsync

Ta właściwość umożliwia korzystanie z stron asynchronicznych. Strony asynchroniczne są omawiane w dalszej części tego modułu.

## <a name="iscallback"></a>IsCallback

Ta właściwość tylko do odczytu zwraca **wartość true** , jeśli strona jest wynikiem wywołania zwrotnego. Wywołania zwrotne są omawiane w dalszej części tego modułu.

## <a name="iscrosspagepostback"></a>IsCrossPagePostBack

Ta właściwość tylko do odczytu zwraca **wartość true** , jeśli strona jest częścią ogłaszania zwrotnego między stronami. Ogłaszanie zwrotne między stronami są omówione w dalszej części tego modułu.

## <a name="items"></a>Items

Zwraca odwołanie do wystąpienia IDictionary, które zawiera wszystkie obiekty przechowywane w kontekście stron. Możesz dodać elementy do tego obiektu IDictionary i będą one dostępne przez cały okres istnienia kontekstu.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostBack

Ta właściwość określa, czy ASP.NET emituje kod JavaScript, który utrzymuje pozycję przewijania stron w przeglądarce po wystąpieniu zwrotnego. (Szczegóły tej właściwości zostały omówione wcześniej w tym module).

## <a name="master"></a>Główne

Ta właściwość tylko do odczytu zwraca odwołanie do wystąpienia MasterPage dla strony, do której zastosowano stronę wzorcową.

## <a name="masterpagefile"></a>MasterPageFile

Pobiera lub ustawia nazwę pliku strony głównej dla strony. Tę właściwość można ustawić tylko w metodzie preinicjowania.

## <a name="maxpagestatefieldlength"></a>MaxPageStateFieldLength

Ta właściwość pobiera lub ustawia maksymalną długość dla stanu stron w bajtach. Jeśli właściwość jest ustawiona na liczbę dodatnią, stan widoku stron zostanie podzielony na wiele ukrytych pól, dzięki czemu nie zostanie przekroczona liczba określonych bajtów. Jeśli właściwość jest liczbą ujemną, stan widoku nie zostanie podzielony na fragmenty.

## <a name="pageadapter"></a>PageAdapter

Zwraca odwołanie do obiektu PageAdapter, który modyfikuje stronę dla przeglądarki żądającej.

## <a name="previouspage"></a>PreviousPage

Zwraca odwołanie do poprzedniej strony w przypadku serwera. transfer lub ogłaszanie zwrotne na wiele stron.

## <a name="skinid"></a>SkinID

Określa skórkę ASP.NET 2,0, która ma zostać zastosowana do strony.

## <a name="stylesheettheme"></a>StyleSheetTheme

Ta właściwość pobiera lub ustawia arkusz stylów, który jest stosowany do strony.

## <a name="templatecontrol"></a>TemplateControl

Zwraca odwołanie do kontrolki zawierającej dla strony.

## <a name="theme"></a>Tematów

Pobiera lub ustawia nazwę motywu ASP.NET 2,0 zastosowanego do strony. Ta wartość musi być ustawiona przed zainicjowaniem metody wstępnej.

## <a name="title"></a>Tytuł

Ta właściwość pobiera lub ustawia tytuł strony uzyskany z nagłówka strony.

## <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

Pobiera lub ustawia ViewStateEncryptionMode strony. Zapoznaj się ze szczegółową dyskusją tej właściwości wcześniej w tym module.

## <a name="new-protected-properties-of-the-page-class"></a>Nowe właściwości chronione klasy Page

Poniżej znajdują się nowe właściwości chronione klasy Page w ASP.NET 2,0.

## <a name="adapter"></a>Adapter

Zwraca odwołanie do ControlAdapter, które renderuje stronę na urządzeniu, które go zażądało.

## <a name="asyncmode"></a>AsyncMode

Ta właściwość wskazuje, czy strona jest przetwarzana asynchronicznie. Jest ona przeznaczona do użycia przez środowisko uruchomieniowe, a nie bezpośrednio w kodzie.

## <a name="clientidseparator"></a>ClientIDSeparator

Ta właściwość zwraca znak używany jako separator podczas tworzenia unikatowych identyfikatorów klientów dla kontrolek. Jest ona przeznaczona do użycia przez środowisko uruchomieniowe, a nie bezpośrednio w kodzie.

## <a name="pagestatepersister"></a>PageStatePersister

Ta właściwość zwraca obiekt PageStatePersister dla strony. Ta właściwość jest używana głównie przez deweloperów formantów ASP.NET.

## <a name="uniquefilepathsuffix"></a>UniqueFilePathSuffix

Ta właściwość zwraca unikatowy sufiks, który jest dołączany do ścieżki pliku dla przeglądarek buforowania. Wartość domyślna to \_\_ufps = i 6-cyfrowy.

## <a name="new-public-methods-for-the-page-class"></a>Nowe metody publiczne dla klasy Page

Następujące metody publiczne są nowe dla klasy Page w ASP.NET 2,0.

## <a name="addonprerendercompleteasync"></a>AddOnPreRenderCompleteAsync

Ta metoda rejestruje delegatów obsługi zdarzeń na potrzeby asynchronicznego wykonywania stron. Strony asynchroniczne są omawiane w dalszej części tego modułu.

## <a name="applystylesheetskin"></a>ApplyStyleSheetSkin

Stosuje właściwości w arkuszu stylów stron na stronie.

## <a name="executeregisteredasynctasks"></a>ExecuteRegisteredAsyncTasks

Ta metoda to zadanie asynchroniczne.

### <a name="getvalidators"></a>Getwalidators

Zwraca kolekcję modułów walidacji dla określonej grupy walidacji lub domyślnej grupy walidacji, jeśli żadna nie została określona.

## <a name="registerasynctask"></a>RegisterAsyncTask

Ta metoda rejestruje nowe zadanie asynchroniczne. Strony asynchroniczne są omówione w dalszej części tego modułu.

## <a name="registerrequirescontrolstate"></a>RegisterRequiresControlState

Ta metoda informuje ASP.NET, że stan formantu strony musi być utrwalony.

## <a name="registerrequiresviewstateencryption"></a>RegisterRequiresViewStateEncryption

Ta metoda informuje ASP.NET, że stan wyświetlania stron wymaga szyfrowania.

## <a name="resolveclienturl"></a>ResolveClientUrl

Zwraca względny adres URL, który może być używany do żądań klientów dla obrazów itd.

## <a name="setfocus"></a>SetFocus

Ta metoda ustawi fokus na kontrolkę określoną podczas początkowego ładowania strony.

## <a name="unregisterrequirescontrolstate"></a>UnregisterRequiresControlState

Ta metoda spowoduje Wyrejestrowanie kontrolki, która jest do niej przenoszona, ponieważ nie wymaga już trwałości stanu kontroli.

## <a name="changes-to-the-page-lifecycle"></a>Zmiany w cyklu życia strony

Cykl życia strony w ASP.NET 2,0 nie zmienił się znacznie, ale istnieją nowe metody, których należy wiedzieć. Cykl życia strony z ASP.NET 2,0 został przedstawiony poniżej.

## <a name="preinit-new-in-aspnet-20"></a>PreInit (Nowość w ASP.NET 2,0)

Wydarzenie wstępne jest najwcześniejszym etapem cyklu życia, do którego ma dostęp Deweloper. Dodanie tego zdarzenia pozwala programowo zmienić motywy ASP.NET 2,0, strony wzorcowe, właściwości dostępu dla profilu ASP.NET 2,0 itd. Jeśli jesteś w stanie ogłaszania zwrotnego, ważne jest, aby pamiętać, że ten stan wyświetlania nie został jeszcze zastosowany do kontroli w tym punkcie cyklu życia. W związku z tym, jeśli deweloper zmieni właściwość kontrolki na tym etapie, prawdopodobnie zostanie nadpisany później w cyklu życia stron.

## <a name="init"></a>Pocz

Zdarzenie init nie zostało zmienione z ASP.NET 1. x. Jest to miejsce, w którym warto odczytać lub zainicjować właściwości formantów na stronie. Na tym etapie strony główne, motywy itp. zostały już zastosowane do strony.

## <a name="initcomplete-new-in-20"></a>InitComplete (Nowość w 2,0)

Zdarzenie InitComplete jest wywoływane na końcu etapu inicjowania stron. W tym momencie cyklu życia można uzyskać dostęp do kontrolek na stronie, ale ich stan nie został jeszcze wypełniony.

## <a name="preload-new-in-20"></a>Wstępne ładowanie (Nowość w 2,0)

To zdarzenie jest wywoływane po zastosowaniu wszystkich danych dotyczących ogłaszania zwrotnego i przed załadowaniem\_strony.

## <a name="load"></a>Załaduj

Zdarzenie ładowania nie zostało zmienione z ASP.NET 1. x.

## <a name="loadcomplete-new-in-20"></a>LoadComplete (Nowość w 2,0)

Zdarzenie LoadComplete jest ostatnim zdarzeniem na etapie ładowania stron. Na tym etapie wszystkie dane zwrotne i stan stanu zostały zastosowane do strony.

## <a name="prerender"></a>PreRender

Jeśli chcesz, aby stan wyświetlania był prawidłowo obsługiwany dla kontrolek, które są dodawane do strony dynamicznie, zdarzenie PreRender jest ostatnią okazją do dodania.

## <a name="prerendercomplete-new-in-20"></a>PreRenderComplete (Nowość w 2,0)

Na etapie PreRenderComplete wszystkie formanty zostały dodane do strony, a strona jest gotowa do renderowania. Zdarzenie PreRenderComplete jest ostatnim zdarzeniem wywoływane przed zapisaniem elementu ViewState stron.

## <a name="savestatecomplete-new-in-20"></a>SaveStateComplete (Nowość w 2,0)

Zdarzenie SaveStateComplete jest wywoływane natychmiast po zapisaniu wszystkich stanów widoku strony i kontroli. Jest to ostatnie zdarzenie przed rzeczywistym renderowaniem strony w przeglądarce.

## <a name="render"></a>Renderowanie

Metoda renderowania nie została zmieniona od ASP.NET 1. x. Jest to miejsce, w którym zainicjowano HtmlTextWriter, a strona jest renderowana w przeglądarce.

## <a name="cross-page-postback-in-aspnet-20"></a>Ogłaszanie zwrotne między stronami w programie ASP.NET 2,0

W ASP.NET 1. x ogłaszanie zwrotne było wymagane do opublikowania na tej samej stronie. Ogłaszanie zwrotne między stronami nie jest dozwolone. ASP.NET 2,0 dodaje możliwość ogłaszania zwrotnego na innej stronie za pośrednictwem interfejsu IButtonControl. Wszystkie kontrolki implementujące nowy interfejs IButtonControl (Button, element LinkButton i kliknięto element ImageButton oprócz niestandardowych kontrolek innych firm) mogą korzystać z tej nowej funkcji przy użyciu atrybutu PostBackUrl. Poniższy kod przedstawia kontrolkę przycisku, która przechodzi z powrotem do drugiej strony.

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample5.aspx)]

Po opublikowaniu strony Strona, która inicjuje ogłaszanie zwrotne, jest dostępna za pośrednictwem właściwości PreviousPage na drugiej stronie. Ta funkcja jest implementowana za pośrednictwem nowego formularza WebForm\_DoPostBackWithOptions funkcji po stronie klienta, która ASP.NET 2,0 renderuje na stronie, gdy kontrolka ponownie zapisuje się na innej stronie. Ta funkcja języka JavaScript jest dostarczana przez nową obsługę WebResource. axd, która emituje skrypt do klienta programu.

Poniżej znajduje się przewodnik po stronie ogłaszania zwrotnego.

![](the-asp-net-2-0-page-model/_static/image2.png)

[Otwórz wideo pełnoekranowe](the-asp-net-2-0-page-model/_static/xpage1.wmv)

## <a name="more-details-on-cross-page-postbacks"></a>Więcej szczegółowych informacji na temat ogłaszania zwrotnego między stronami

### <a name="viewstate"></a>Stanu

Być może zażądano, aby wcześniej zaprosił o to, co się dzieje ze stanem wyświetlania na pierwszej stronie scenariusza ogłaszania zwrotnego na stronie. Po wykonaniu tej operacji każda kontrolka, która nie implementuje IPostBackDataHandler, będzie utrzymywać swój stan za pośrednictwem elementu ViewState, dlatego w celu uzyskania dostępu do właściwości tej kontrolki na drugiej stronie odświeżenia zwrotnego na stronie należy mieć dostęp do stanu wyświetlania strony. ASP.NET 2,0 zajmuje się tym scenariuszem przy użyciu nowego pola ukrytego na drugiej stronie o nazwie \_\_PREVIOUSPAGE. Pole formularza \_\_PREVIOUSPAGE zawiera stan wyświetlania pierwszej strony, aby można było uzyskać dostęp do właściwości wszystkich formantów na drugiej stronie.

### <a name="circumventing-findcontrol"></a>Obejście FindControl

W instruktażu wideo dla wielostronicowego ogłaszania zwrotnego użyto metody FindControl, aby uzyskać odwołanie do kontrolki TextBox na pierwszej stronie. Ta metoda dobrze sprawdza się w tym celu, ale FindControl jest kosztowna i wymaga napisania dodatkowego kodu. Na szczęście ASP.NET 2,0 stanowi alternatywę dla FindControl w tym celu, która będzie działała w wielu scenariuszach. Dyrektywa PreviousPageType pozwala na posiadanie silnie określonego odwołania do poprzedniej strony przy użyciu atrybutu TypeName lub VirtualPath. Atrybut TypeName pozwala określić typ poprzedniej strony, podczas gdy atrybut VirtualPath umożliwia odwoływanie się do poprzedniej strony przy użyciu ścieżki wirtualnej. Po ustawieniu dyrektywy PreviousPageType należy uwidocznić kontrolki itp., na które chcesz zezwolić na dostęp za pomocą właściwości publicznych.

## <a name="lab-1-cross-page-postback"></a>Funkcja ogłaszania zwrotnego na stronie laboratorium 1

W tym laboratorium utworzysz aplikację korzystającą z nowej funkcji ogłaszania zwrotnego na stronie ASP.NET 2,0.

1. Otwórz program Visual Studio 2005 i Utwórz nową witrynę sieci Web ASP.NET.
2. Dodaj nowy formularz WebForm o nazwie PAGE2. aspx.
3. Otwórz wartość default. aspx w widok Projekt i Dodaj kontrolkę Button i kontrolkę TextBox. 

    1. Nadaj przyciskowi formantowi identyfikator **SubmitButton** , a pole tekstowe kontroluje identyfikator **użytkownika**.
    2. Ustaw właściwość PostBackUrl przycisku na PAGE2. aspx.
4. Otwórz PAGE2. aspx w widoku źródła.
5. Dodaj dyrektywę @ PreviousPageType, jak pokazano poniżej:
6. Dodaj następujący kod na stronie\_ładowanie kodu PAGE2. aspx w tle: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample6.cs)]
7. Skompiluj projekt, klikając polecenie Kompiluj w menu Kompilacja.
8. Dodaj do kodu Poniższy kod dla default. aspx: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample7.cs)]
9. Zmień\_ładowania strony w PAGE2. aspx na następujące: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample8.cs)]
10. Skompiluj projekt.
11. Uruchom projekt.
12. Wprowadź swoją nazwę w polu tekstowym, a następnie kliknij przycisk.
13. Co to jest wynik?

## <a name="asynchronous-pages-in-aspnet-20"></a>Strony asynchroniczne w ASP.NET 2,0

Wiele problemów z rywalizacją w ASP.NET jest spowodowanych opóźnieniami wywołań zewnętrznych (na przykład wywołaniami usługi sieci Web lub bazą danych), opóźnieniami we/wy plików itp. Gdy żądanie jest nawiązywane z aplikacją ASP.NET, ASP.NET używa jednego z jego wątków roboczych do obsługi tego żądania. To żądanie jest właścicielem tego wątku do momentu ukończenia żądania i wysłania odpowiedzi. ASP.NET 2,0 poszukuje problemów opóźnienia z tymi typami problemów, dodając funkcję do wykonywania stron asynchronicznie. Oznacza to, że wątek roboczy może uruchomić żądanie, a następnie przełączać dodatkowe wykonywanie do innego wątku, co umożliwia szybkie zwrócenie do dostępnej puli wątków. Po zakończeniu operacji we/wy pliku, wywołaniu bazy danych itp. zostanie pobrany nowy wątek z puli wątków, aby zakończyć żądanie.

Pierwszym krokiem w przypadku wykonywania strony asynchronicznej jest ustawienie atrybutu **Async** dyrektywy Page, na przykład:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample9.aspx)]

Ten atrybut informuje ASP.NET o implementacji IHttpAsyncHandler dla strony.

Następnym krokiem jest wywołanie metody AddOnPreRenderCompleteAsync w punkcie cyklu życia strony przed zdarzeniem PreRender. (Ta metoda jest zwykle wywoływana na stronie\_Load). Metoda AddOnPreRenderCompleteAsync przyjmuje dwa parametry: BeginEventHandler i EndEventHandler. BeginEventHandler zwraca element IAsyncResult, który następnie jest przekazany jako parametr do EndEventHandler.

Poniższy film przedstawia przewodnik po asynchronicznym żądaniu strony.

![](the-asp-net-2-0-page-model/_static/image3.png)

[Otwórz wideo pełnoekranowe](the-asp-net-2-0-page-model/_static/async1.wmv)

> [!NOTE]
> Strona asynchroniczna nie jest renderowana w przeglądarce, dopóki nie zostanie ukończona EndEventHandler. Brak wątpliwości, ale niektórzy deweloperzy będą traktować żądania asynchroniczne jako podobne do asynchronicznych wywołań zwrotnych. Należy pamiętać, że nie są one. Korzyść dla żądań asynchronicznych polega na tym, że pierwszy wątek roboczy może zostać zwrócony do puli wątków w celu obsługi nowych żądań, co zmniejsza rywalizację z powodu powiązania operacji we/wy itp.

## <a name="script-callbacks-in-aspnet-20"></a>Wywołania zwrotne skryptu w ASP.NET 2,0

Deweloperzy sieci Web zawsze szukali sposobów zapobiegania migotaniu związanym z wywołaniem zwrotnym. W ASP.NET 1. x SmartNavigation była najbardziej typowa Metoda unikania migotania, ale SmartNavigation powodowała problemy dla niektórych deweloperów ze względu na złożoność jego wdrożenia na kliencie. ASP.NET 2,0 rozwiązuje ten problem z wywołaniami zwrotnymi skryptu. Wywołania zwrotne skryptu używają XMLHTTP do żądania serwera sieci Web za pośrednictwem języka JavaScript. Żądanie XMLHTTP zwraca dane XML, które można następnie manipulować za pośrednictwem modelu DOM przeglądarki. Kod XMLHTTP jest ukryty przez użytkownika przez nową procedurę obsługi WebResource. axd.

Istnieje kilka kroków, które są niezbędne do skonfigurowania wywołania zwrotnego skryptu w ASP.NET 2,0.

## <a name="step-1--implement-the-icallbackeventhandler-interface"></a>Krok 1. Implementowanie interfejsu ICallbackEventHandler

Aby ASP.NET rozpoznawać stronę jako udział w wywołaniu zwrotnym skryptu, należy zaimplementować interfejs ICallbackEventHandler. Można to zrobić w pliku związanym z kodem, takim jak:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample10.cs)]

Można to zrobić również przy użyciu dyrektywy @ Implements, takiej jak:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample11.aspx)]

W przypadku korzystania z kodu ASP.NET śródwierszowego zazwyczaj należy używać dyrektywy @ Implements.

## <a name="step-2--call-getcallbackeventreference"></a>Krok 2: wywołanie GetCallbackEventReference

Jak wspomniano wcześniej, wywołanie XMLHTTP jest hermetyzowane w obsłudze WebResource. axd. Gdy strona jest renderowana, ASP.NET doda wywołanie do formularza WebForm\_DoCallback, skryptu klienta dostarczonego przez WebResource. axd. \_funkcja DoCallback zastępuje funkcję \_\_doPostBack dla wywołania zwrotnego. Należy pamiętać, że \_\_doPostBack programowo przesyła formularz na stronie. W scenariuszu wywołania zwrotnego chcesz zapobiec ogłaszaniu zwrotnego, więc \_\_doPostBack nie wystarczą.

> [!NOTE]
> \_\_doPostBack jest nadal renderowane na stronie w scenariuszu wywołania zwrotnego skryptu klienta. Nie jest on jednak używany do wywołania zwrotnego.

Argumenty elementu WebForm\_DoCallback po stronie klienta są udostępniane za pośrednictwem funkcji po stronie serwera GetCallbackEventReference, która zwykle jest wywoływana podczas ładowania strony\_. Typowe wywołanie GetCallbackEventReference może wyglądać następująco:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample12.cs)]

> [!NOTE]
> W takim przypadku cm jest wystąpieniem ClientScriptManager. Klasa ClientScriptManager zostanie uwzględniona w dalszej części tego modułu.

Istnieje kilka przeciążonych wersji programu GetCallbackEventReference. W takim przypadku argumenty są następujące:

`this`

Odwołanie do kontrolki, w której jest wywoływana GetCallbackEventReference. W tym przypadku jest to sama strona.

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample13.js)]

Argument ciągu, który będzie przekazywać z kodu po stronie klienta do zdarzenia po stronie serwera. W takim przypadku błyskawiczne przekazanie wartości listy rozwijanej o nazwie ddlCompany.

`ShowCompanyName`

Nazwa funkcji po stronie klienta, która będzie akceptować wartość zwracaną (jako ciąg) ze zdarzenia wywołania zwrotnego po stronie serwera. Ta funkcja zostanie wywołana tylko wtedy, gdy wywołanie zwrotne po stronie serwera zakończy się pomyślnie. W związku z tym, ogólnie zaleca się użycie przeciążonej wersji GetCallbackEventReference, która pobiera dodatkowy argument ciągu określający nazwę funkcji po stronie klienta do wykonania w przypadku błędu.

`null`

Ciąg reprezentujący funkcję po stronie klienta, która została zainicjowana przed wywołaniem zwrotnym do serwera. W tym przypadku nie ma takiego skryptu, więc argument ma wartość null.

`true`

Wartość logiczna określająca, czy należy przeprowadzić asynchroniczne wywołanie zwrotne.

Wywołanie do WebForm\_DoCallback na kliencie przekaże te argumenty. W związku z tym, gdy ta strona jest renderowana na kliencie, ten kod będzie wyglądać następująco:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample14.js)]

Zwróć uwagę, że podpis funkcji na kliencie jest nieco inny. Funkcja po stronie klienta przekazuje 5 ciągów i wartość logiczną. Dodatkowy ciąg (który ma wartość null w powyższym przykładzie) zawiera funkcję po stronie klienta, która będzie obsługiwać wszystkie błędy wywołania zwrotnego po stronie serwera.

## <a name="step-3--hook-the-client-side-control-event"></a>Krok 3. przełączenie zdarzenia kontroli po stronie klienta

Zwróć uwagę, że wartość zwracana GetCallbackEventReference powyżej została przypisana do zmiennej ciągu. Ten ciąg jest używany do podłączania zdarzenia po stronie klienta dla kontrolki inicjującej wywołanie zwrotne. W tym przykładzie wywołanie zwrotne jest inicjowane przez listę rozwijaną na stronie, więc Chcę podłączyć zdarzenie *OnChange* .

Aby podłączyć zdarzenie po stronie klienta, wystarczy dodać program obsługi do znacznika po stronie klienta w następujący sposób:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample15.cs)]

Odwołaj to *cbRef* jest wartością zwracaną z wywołania do GetCallbackEventReference. Zawiera wywołanie metody WebForm\_DoCallback, która została pokazana powyżej.

## <a name="step-4--register-the-client-side-script"></a>Krok 4. rejestrowanie skryptu po stronie klienta

Odwołaj, że wywołanie GetCallbackEventReference określa, że skrypt po stronie klienta o nazwie **ShowCompanyName** zostałby wykonany, gdy wywołanie zwrotne po stronie serwera zostanie wykonane pomyślnie. Ten skrypt należy dodać do strony przy użyciu wystąpienia ClientScriptManager. (Klasa ClientScriptManager zostanie omówiona w dalszej części tego modułu). Można to zrobić w następujący sposób:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample16.js)]

## <a name="step-5--call-the-methods-of-the-icallbackeventhandler-interface"></a>Krok 5. wywoływanie metod interfejsu ICallbackEventHandler

ICallbackEventHandler zawiera dwie metody, które należy zaimplementować w kodzie. Są one **RaiseCallbackEvent** i **GetCallbackEvent**.

**RaiseCallbackEvent** przyjmuje ciąg jako argument i zwraca wartość Nothing. Argument ciągu jest przesyłany z wywołania po stronie klienta do formularza WebForm\_DoCallback. W tym przypadku ta wartość jest atrybutem *wartości* listy rozwijanej o nazwie ddlCompany. Kod po stronie serwera powinien być umieszczony w metodzie RaiseCallbackEvent. Na przykład jeśli wywołanie zwrotne powoduje utworzenie żądania Webw odniesieniu do zasobu zewnętrznego, ten kod powinien zostać umieszczony w RaiseCallbackEvent.

**GetCallbackEvent** jest odpowiedzialny za przetwarzanie powrotu wywołania zwrotnego do klienta. Nie przyjmuje argumentów i zwraca ciąg. Ciąg, który zwraca zostanie przesłany jako argument do funkcji po stronie klienta, w tym przypadku *ShowCompanyName*.

Po wykonaniu powyższych kroków możesz przystąpić do wykonania wywołania zwrotnego skryptu w ASP.NET 2,0.

![](the-asp-net-2-0-page-model/_static/image4.png)

[Otwórz wideo pełnoekranowe](the-asp-net-2-0-page-model/_static/callback1.wmv)

Wywołania zwrotne skryptu w programie ASP.NET są obsługiwane w dowolnej przeglądarce, która obsługuje wykonywanie wywołań XMLHTTP. Obejmuje ona wszystkie nowoczesne przeglądarki używane dzisiaj. Program Internet Explorer używa obiektu ActiveX XMLHTTP, a inne nowoczesne przeglądarki (w tym nadchodzące IE 7) używają wewnętrznego obiektu XMLHTTP. Aby programowo określić, czy przeglądarka obsługuje wywołania zwrotne, można użyć właściwości **Request. browser. SupportCallback** . Ta właściwość zwróci **wartość true** , jeśli klient żądający obsługuje wywołania zwrotne skryptu.

## <a name="working-with-client-script-in-aspnet-20"></a>Praca z skryptem klienta w programie ASP.NET 2,0

Skrypty klienta w programie ASP.NET 2,0 są zarządzane przy użyciu klasy ClientScriptManager. Klasa ClientScriptManager śledzi skrypty klienta przy użyciu typu i nazwy. Zapobiega to programistycznemu wstawieniu tego samego skryptu na stronie więcej niż jeden raz.

> [!NOTE]
> Po pomyślnym zarejestrowaniu skryptu na stronie każda kolejna próba zarejestrowania tego samego skryptu spowoduje, że skrypt nie zostanie zarejestrowany po raz drugi. Nie dodano zduplikowanych skryptów i nie wystąpił żaden wyjątek. Aby uniknąć niepotrzebnych obliczeń, istnieją metody, których można użyć do określenia, czy skrypt jest już zarejestrowany, aby nie próbować go zarejestrować więcej niż raz.

Metody ClientScriptManager powinny być znane wszystkim bieżącym deweloperom ASP.NET:

## <a name="registerclientscriptblock"></a>RegisterClientScriptBlock

Ta metoda dodaje skrypt na początku renderowanej strony. Jest to przydatne w przypadku dodawania funkcji, które będą jawnie wywoływane na kliencie.

Istnieją dwie przeciążone wersje tej metody. Trzy z czterech argumentów są wspólne między nimi. Oto one:

`type (string)`

Argument ***typu*** identyfikuje typ skryptu. Zwykle dobrym pomysłem jest użycie typu strony (to jest. GetType ()) dla typu.

`key (string)`

Argument ***klucza*** jest kluczem zdefiniowanym przez użytkownika dla skryptu. Ta powinna być unikatowa dla każdego skryptu. Jeśli podjęto próbę dodania skryptu z tym samym kluczem i typem już dodanego skryptu, nie zostanie on dodany.

`script (string)`

Argument ***skryptu*** jest ciągiem zawierającym rzeczywisty skrypt do dodania. Zaleca się użycie elementu StringBuilder do utworzenia skryptu, a następnie użycie metody ToString () w StringBuilder do przypisania argumentu ***skryptu*** .

Jeśli używasz przeciążonego RegisterClientScriptBlock, który przyjmuje tylko trzy argumenty, musisz dołączyć elementy skryptu (&lt;skrypt&gt; i &lt;/Script&gt;) w skrypcie.

Możesz wybrać użycie przeciążenia RegisterClientScriptBlock, które przyjmuje czwarty argument. Czwarty argument to wartość logiczna określająca, czy ASP.NET powinna dodawać elementy skryptu. Jeśli ten argument ma **wartość true**, skrypt nie powinien zawierać jawnie elementów skryptu.

Użyj metody IsClientScriptBlockRegistered, aby określić, czy skrypt został już zarejestrowany. Pozwala to uniknąć próby ponownego zarejestrowania skryptu, który został już zarejestrowany.

### <a name="registerclientscriptinclude-new-in-20"></a>RegisterClientScriptInclude (Nowość w 2,0)

Tag RegisterClientScriptInclude tworzy blok skryptu, który łączy się z zewnętrznym plikiem skryptu. Ma dwa przeciążenia. Jeden z nich przyjmuje klucz i adres URL. Drugi dodaje trzeci argument określający typ.

Na przykład poniższy kod generuje blok skryptu, który łączy się z jsfunctions. js w folderze głównym folderu skrypty aplikacji:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample17.cs)]

Ten kod generuje następujący kod na renderowanej stronie:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample18.html)]

> [!NOTE]
> Blok skryptu jest renderowany w dolnej części strony.

Użyj metody IsClientScriptIncludeRegistered, aby określić, czy skrypt został już zarejestrowany. Pozwala to uniknąć próby ponownego zarejestrowania skryptu.

## <a name="registerstartupscript"></a>RegisterStartupScript

Metoda RegisterStartupScript przyjmuje te same argumenty co Metoda RegisterClientScriptBlock. Skrypt zarejestrowany w RegisterStartupScript jest wykonywany po załadowaniu strony, ale przed zdarzeniem OnLoad po stronie klienta. W 1. X skrypty zarejestrowane za pomocą RegisterStartupScript zostały umieszczone tuż przed tagiem zamykającym&gt; &lt;formularzy, podczas gdy skrypty zarejestrowane w RegisterClientScriptBlock zostały umieszczone bezpośrednio po tagu otwierającym &lt;&gt;. W ASP.NET 2,0 oba są umieszczane bezpośrednio przed zamykanym tagiem &lt;formularzy&gt;.

> [!NOTE]
> Jeśli zarejestrujesz funkcję z RegisterStartupScript, ta funkcja nie zostanie wykonana, dopóki nie zostanie jawnie wywołana w kodzie po stronie klienta.

Użyj metody IsStartupScriptRegistered, aby określić, czy skrypt został już zarejestrowany, i unikaj próby ponownego zarejestrowania skryptu.

## <a name="other-clientscriptmanager-methods"></a>Inne metody ClientScriptManager

Oto niektóre z innych użytecznych metod klasy ClientScriptManager.

|  <strong>GetCallbackEventReference</strong>   |                                                 Zobacz wywołania zwrotne skryptów wcześniej w tym module.                                                 |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|  <strong>GetPostBackClientHyperlink</strong>  |                Pobiera odwołanie JavaScript (JavaScript:&lt;Call&gt;), którego można użyć do ogłaszania zwrotnego ze zdarzenia po stronie klienta.                 |
|  <strong>GetPostBackEventReference</strong>   |                                   Pobiera ciąg, którego można użyć do zainicjowania wpisu z powrotem od klienta.                                    |
|      <strong>GetWebResourceUrl</strong>       | Zwraca adres URL do zasobu osadzonego w zestawie. Musi być używany w połączeniu z <strong>RegisterClientScriptResource</strong>. |
| <strong>RegisterClientScriptResource</strong> |     Rejestruje zasób sieci Web na stronie. Są to zasoby osadzone w zestawie i obsługiwane przez nową procedurę obsługi WebResource. axd.      |
|     <strong>RegisterHiddenField</strong>      |                                                 Rejestruje ukryte pole formularza ze stroną.                                                 |
|  <strong>RegisterOnSubmitStatement</strong>   |                                  Rejestruje kod po stronie klienta, który jest wykonywany podczas przesyłania formularza HTML.                                   |

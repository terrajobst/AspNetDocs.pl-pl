---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
title: Informacje na temat aktualizacji stron częściowych przy użyciu technologii ASP.NET AJAX | Microsoft Docs
author: scottcate
description: Prawdopodobnie najbardziej widoczną funkcją rozszerzeń AJAX ASP.NET jest możliwość przeprowadzania aktualizacji stron częściowej lub przyrostowej bez wykonywania pełnego ogłaszania zwrotnego w usłudze t...
ms.author: riande
ms.date: 03/28/2008
ms.assetid: 54d9df99-1161-4899-b4e8-2679c85915e7
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
msc.type: authoredcontent
ms.openlocfilehash: 4b87cb8f58dbd7f27b16bcb0d488ff361770d4fe
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78545970"
---
# <a name="understanding-partial-page-updates-with-aspnet-ajax"></a>Objaśnienie aktualizacji stron częściowych przy użyciu rozszerzeń ASP.NET AJAX

przez [Scott Cate](https://github.com/scottcate)

[Pobierz plik PDF](https://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial01_Partial_Page_Updates_cs.pdf)

> Prawdopodobnie najbardziej widoczną funkcją rozszerzeń AJAX ASP.NET jest możliwość przeprowadzania aktualizacji stron częściowej lub przyrostowej bez konieczności pełnego ogłaszania zwrotnego na serwerze i braku zmian w kodzie i minimalnej liczby zmian znaczników. Zalety są obszerne — stan multimediów (np. Adobe Flash lub Windows Media) nie jest zmieniany, koszty przepustowości są skracane, a klient nie korzysta z migotania zwykle powiązanego z ogłaszaniem zwrotnym.

## <a name="introduction"></a>Wprowadzenie

Technologia ASP.NET firmy Microsoft zapewnia model programowania zorientowany obiektowo i oparty na zdarzeniach oraz kieruje go do zalet skompilowanego kodu. Jednak model przetwarzania po stronie serwera ma kilka wad związanych z technologią:

- Aktualizacje strony wymagają przeprowadzenia rundy do serwera, który wymaga odświeżenia strony.
- Przeprowadzenie rundy nie utrwala żadnych efektów wygenerowanych przez JavaScript lub inną technologię po stronie klienta (na przykład Adobe Flash).
- Podczas ogłaszania zwrotnego przeglądarki inne niż Microsoft Internet Explorer nie obsługują automatycznego przywracania pozycji przewijania. Nawet w programie Internet Explorer nadal występuje migotanie, ponieważ strona jest odświeżana.
- Ogłaszanie zwrotne może wiązać się z dużą ilością przepustowości, ponieważ pole formularza \_\_jest rozrastane, szczególnie podczas pracy z kontrolkami, takimi jak kontrolka GridView lub powtórzenia.
- Nie istnieje ujednolicony model służący do uzyskiwania dostępu do usług sieci Web za poorednictwem języka JavaScript lub innej technologii po stronie klienta.

Wprowadź rozszerzenia ASP.NET AJAX firmy Microsoft. Technologia AJAX, która oznacza **synchroniczną** AvaScriptę **J** **X** ml, jest zintegrowaną platformą umożliwiającą udostępnianie przyrostowych aktualizacji stron za pośrednictwem **JavaScript na wielu** platformach, składającą się z kodu po stronie serwera składającego się z technologii Microsoft Ajax Framework oraz składnika skryptu zwanego biblioteką skryptów Microsoft Ajax. Rozszerzenia AJAX ASP.NET zapewniają również obsługę wielu platform na potrzeby uzyskiwania dostępu do usług sieci Web ASP.NET za pośrednictwem języka JavaScript.

Ten oficjalny dokument analizuje funkcje częściowej aktualizacji strony rozszerzeń ASP.NET AJAX, które obejmują składnik ScriptManager, formant UpdatePanel i kontrolkę UpdateProgress, i uwzględniają scenariusze, w których powinny lub nie powinny być używane.

Niniejszy dokument jest oparty na wersji beta 2 programu Visual Studio 2008 i .NET Framework 3,5, która integruje rozszerzenia AJAX ASP.NET z biblioteką klas bazowych (w którym wcześniej był dostępny składnik dodatku dla ASP.NET 2,0). W tym dokumencie przyjęto również założenie, że używasz programu Visual Studio 2008, a nie programu Visual Web Developer Express Edition. Niektóre szablony projektów, do których istnieją odwołania, mogą nie być dostępne dla użytkowników programu Visual Web Developer Express.

## <a name="partial-page-updates"></a>Aktualizacje strony częściowej

Prawdopodobnie najbardziej widoczną funkcją rozszerzeń AJAX ASP.NET jest możliwość przeprowadzania aktualizacji stron częściowej lub przyrostowej bez konieczności pełnego ogłaszania zwrotnego na serwerze i braku zmian w kodzie i minimalnej liczby zmian znaczników. Zalety są rozległe — stan multimediów (np. Adobe Flash lub Windows Media) nie jest zmieniany, koszty przepustowości są skracane, a klient nie korzysta z migotania zwykle powiązanego z ogłaszaniem zwrotnym.

Możliwość integracji renderowania częściowej strony jest zintegrowana z ASP.NET z minimalnymi zmianami w projekcie.

## <a name="walkthrough-integrating-partial-rendering-into-an-existing-project"></a>Wskazówki: Integrowanie renderowania częściowego z istniejącym projektem

1. W Microsoft Visual Studio 2008 Utwórz nowy projekt witryny sieci Web ASP.NET, przechodząc do pozycji <em>plik</em> <em>-&gt; nowej</em> <em>-&gt; witrynę sieci Web</em> i wybierając ASP.NET witrynę sieci Web z okna dialogowego. Można ją nazwać w dowolny sposób i można ją zainstalować w systemie plików lub w Internet Information Services (IIS).
2. Zostanie wyświetlona pusta strona domyślna ze znacznikiem Basic ASP.NET (formularz po stronie serwera i dyrektywa `@Page`). Usuń etykietę o nazwie `Label1` i przycisk o nazwie `Button1` na stronie w elemencie form. Możesz ustawić własne właściwości tekstu.
3. W widok Projekt kliknij dwukrotnie pozycję `Button1`, aby wygenerować program obsługi zdarzeń związany z kodem. W ramach tego programu obsługi zdarzeń Ustaw `Label1.Text` kliknięciem przycisku! .

**Lista 1: znacznik default. aspx przed włączeniem częściowego renderowania**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample1.aspx)]

**Lista 2: CodeBehind (przycięta) w default.aspx.cs**

[!code-csharp[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample2.cs)]

1. Naciśnij klawisz F5, aby uruchomić witrynę sieci Web. Program Visual Studio wyświetli monit o dodanie pliku Web. config w celu włączenia debugowania. zrób tak. Po kliknięciu przycisku Zauważ, że strona Odświeża stronę w celu zmiany tekstu w etykiecie, a po ponownym narysowaniu strony występuje krótkie migotanie.
2. Po zamknięciu okna przeglądarki Wróć do programu Visual Studio i na stronę znaczników. Przewiń w dół w przyborniku programu Visual Studio i Znajdź kartę z etykietą rozszerzenia AJAX. (Jeśli nie masz tej karty, ponieważ używasz starszej wersji rozszerzeń AJAX lub Atlas, zapoznaj się z przewodnikiem dotyczącym rejestrowania elementów przybornika rozszerzeń AJAX w dalszej części tego dokumentu lub zainstaluj bieżącą wersję z Instalator Windows do pobrania z witryny sieci Web).

[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image2.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image1.png)

([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](understanding-partial-page-updates-with-asp-net-ajax/_static/image3.png))

1. <em>Znany problem:</em> W przypadku zainstalowania programu Visual Studio 2008 na komputerze, na którym jest już zainstalowany program Visual Studio 2005 z rozszerzeniami ASP.NET 2,0 AJAX, program Visual Studio 2008 zaimportuje elementy przybornika rozszerzeń AJAX. Możesz określić, czy jest to przypadek, sprawdzając etykietkę narzędzia składników; powinny one być napisane w wersji 3.5.0.0. Jeśli poznamy wersję 2.0.0.0, zaimportowano stare elementy przybornika i będzie konieczne ręczne zaimportowanie ich przy użyciu okna dialogowego Wybierz elementy przybornika w programie Visual Studio. Nie będzie można dodać kontrolek w wersji 2 za pomocą projektanta.

2. Przed rozpoczęciem tagu `<asp:Label>` Utwórz wiersz odstępu, a następnie kliknij dwukrotnie formant UpdatePanel w przyborniku. Należy zauważyć, że nowa `@Register` dyrektywa jest uwzględniona u góry strony, co oznacza, że kontrolki w przestrzeni nazw System. Web. UI powinny być importowane przy użyciu prefiksu `asp:`.
3. Przeciągnij znacznik zamykania `</asp:UpdatePanel>` poza końcem elementu Button, dzięki czemu element jest poprawnie sformułowany z kontrolkami etykiet i przycisków.
4. Po tagu otwierającym `<asp:UpdatePanel>` Rozpocznij otwieranie nowego tagu. Należy pamiętać, że technologia IntelliSense poprosi o dwie opcje. W takim przypadku Utwórz tag `<ContentTemplate>`. Pamiętaj, aby otoczyć ten tag etykietą i przyciskiem, aby znaczniki były poprawnie sformułowane.

[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image5.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image4.png)

([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](understanding-partial-page-updates-with-asp-net-ajax/_static/image6.png))

1. W dowolnym miejscu elementu `<form>` należy dodać kontrolkę ScriptManager przez dwukrotne kliknięcie elementu `ScriptManager` w przyborniku.
2. Edytuj znacznik `<asp:ScriptManager>` tak, aby zawierał `EnablePartialRendering= true`atrybutu.

**Lista 3: Adiustacja dla default. aspx z włączonym renderowaniem częściowym**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample3.aspx)]

1. Otwórz plik Web. config. Zwróć uwagę, że program Visual Studio automatycznie dodał odwołanie do kompilacji do System. Web. Extensions. dll.

1. Co nowego w programie Visual Studio 2008: plik Web. config, który jest dostarczany z szablonami projektu witryny sieci Web ASP.NET automatycznie zawiera wszystkie niezbędne odwołania do rozszerzeń ASP.NET AJAX, a także zawiera sekcje z informacjami o konfiguracji, które mogą być Cofnij komentarz, aby włączyć dodatkowe funkcje. Program Visual Studio 2005 ma podobne szablony, gdy zainstalowano rozszerzenia ASP.NET 2,0 AJAX. Jednak w programie Visual Studio 2008 rozszerzenia AJAX są domyślnie wyłączane (oznacza to, że są one domyślnie przywoływane, ale można je usunąć jako odwołania).

[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image8.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image7.png)

([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](understanding-partial-page-updates-with-asp-net-ajax/_static/image9.png))

1. Naciśnij klawisz F5, aby uruchomić witrynę internetową. Zwróć uwagę, jak nie są wymagane żadne zmiany kodu źródłowego do obsługi renderowania częściowego — oznaczenie tylko adiustacji zostało zmienione.

Po uruchomieniu witryny sieci Web powinno być widoczne, że funkcja renderowania częściowego jest teraz włączona, ponieważ po kliknięciu przycisku nie będzie migotać, ani nie będzie żadnych zmian w pozycji przewijania strony (ten przykład nie pokazuje tego). Jeśli po kliknięciu przycisku wyszukasz renderowane Źródło strony, zostanie potwierdzone, że w rzeczywistości nie przeprowadzono publikacji. oryginalny tekst etykiety jest nadal częścią znacznika źródłowego, a etykieta została zmieniona za pomocą języka JavaScript.

W programie Visual Studio 2008 nie jest wyświetlany wstępnie zdefiniowany szablon dla witryny sieci Web z obsługą technologii AJAX ASP.NET. Jednak taki szablon był dostępny w programie Visual Studio 2005, jeśli zainstalowano rozszerzenia Visual Studio 2005 i ASP.NET 2,0 AJAX. W związku z tym skonfigurowanie witryny sieci Web i rozpoczęcie od szablonu witryny sieci Web z obsługą technologii AJAX prawdopodobnie będzie jeszcze łatwiejsze, ponieważ szablon powinien zawierać w pełni skonfigurowany plik Web. config (obsługujący wszystkie rozszerzenia AJAX ASP.NET, w tym dostęp do usług sieci Web). i Serializacja JSON-JavaScript Object Notation) i domyślnie zawiera element UpdatePanel i ContentTemplate na głównej stronie formularzy sieci Web. Włączenie renderowania częściowego przy użyciu tej domyślnej strony jest tak proste jak w kroku 10 tego instruktażu i upuszczenie formantów na stronie.

## <a name="the-scriptmanager-control"></a>Kontrolka ScriptManager

## <a name="scriptmanager-control-reference"></a>Dokumentacja kontrolki ScriptManager

Właściwości z włączoną obsługą znaczników:

| **Nazwa właściwości** | **Typ** | **Opis** |
| --- | --- | --- |
| AllowCustomErrors-Redirect | Bool | Określa, czy w pliku Web. config ma być używana sekcja błędu niestandardowego do obsługi błędów. |
| AsyncPostBackError-Message | Ciąg | Pobiera lub ustawia komunikat o błędzie wysyłany do klienta w przypadku zgłoszenia błędu. |
| AsyncPostBack-Timeout | Int32 | Pobiera lub ustawia domyślną ilość czasu, przez który klient powinien czekać na ukończenie żądania asynchronicznego. |
| EnableScript-Globalization | Bool | Pobiera lub ustawia czy jest włączone globalizacja skryptu. |
| EnableScript-Localization | Bool | Pobiera lub ustawia, czy lokalizacja skryptu jest włączona. |
| ScriptLoadTimeout | Int32 | Określa liczbę sekund, przez jaką można załadować skrypty do klienta programu. |
| ScriptMode | Wyliczenie (Auto, Debug, Release, Inherit) | Pobiera lub ustawia, czy mają być renderowane wersje skryptów |
| ScriptPath | Ciąg | Pobiera lub ustawia ścieżkę katalogu głównego do lokalizacji plików skryptów, które mają być wysłane do klienta. |

Właściwości tylko do kodu:

| **Nazwa właściwości** | **Typ** | **Opis** |
| --- | --- | --- |
| AuthenticationService | AuthenticationService-Manager | Pobiera szczegółowe informacje o serwerze proxy usługi ASP.NET Authentication, który zostanie wysłany do klienta programu. |
| IsDebuggingEnabled | Bool | Pobiera czy debugowanie skryptu i kodu jest włączone. |
| IsInAsyncPostback | Bool | Pobiera czy strona jest obecnie w asynchronicznym żądaniu post. |
| ProfileService | ProfileService-Manager | Pobiera szczegółowe informacje o serwerze proxy usługi profilowania ASP.NET, który zostanie wysłany do klienta. |
| Scripts | Kolekcja&lt;skrypt-dokumentacja&gt; | Pobiera kolekcję odwołań do skryptów, które zostaną wysłane do klienta. |
| Usługi | &lt;usługi kolekcji — dokumentacja&gt; | Pobiera kolekcję odwołań serwera proxy usługi sieci Web, które zostaną wysłane do klienta. |
| SupportsPartialRendering | Bool | Pobiera czy bieżący klient obsługuje renderowanie częściowe. Jeśli ta właściwość zwraca **wartość false**, wszystkie żądania stron będą standardowe ogłaszanie zwrotne. |

Metody kodu publicznego:

| **Nazwa metody** | **Typ** | **Opis** |
| --- | --- | --- |
| SetFocus (ciąg) | Pozycję | Ustawia fokus klienta na określoną kontrolkę, gdy żądanie zostało zakończone. |

Elementy potomne znaczników:

| **Tag** | **Opis** |
| --- | --- |
| &lt;AuthenticationService&gt; | Zawiera szczegółowe informacje o serwerze proxy usługi uwierzytelniania ASP.NET. |
| &lt;ProfileService&gt; | Zawiera szczegółowe informacje o serwerze proxy w usłudze profilowania ASP.NET. |
| &lt;Skrypty&gt; | Zawiera dodatkowe odwołania do skryptów. |
| &lt;ASP: odwołanie do skryptu&gt; | Oznacza odwołanie do określonego skryptu. |
| &lt;Usługa&gt; | Zawiera dodatkowe odwołania usługi sieci Web, które będą miały klasy proxy. |
| &lt;ASP: ServiceReference&gt; | Wskazuje określone odwołanie do usługi sieci Web. |

Formant ScriptManager jest podstawowym podstawą dla rozszerzeń ASP.NET AJAX. Zapewnia dostęp do biblioteki skryptów (w tym rozbudowanego systemu typu skryptu po stronie klienta), obsługuje renderowanie częściowe i zapewnia szeroką pomoc techniczną dla dodatkowych usług ASP.NET (takich jak uwierzytelnianie i profilowanie, ale również inne usługi sieci Web). Kontrolka ScriptManager udostępnia także globalizację i obsługę lokalizacji skryptów klienta.

## <a name="providing-alternative-and-supplemental-scripts"></a>Zapewnianie alternatywnych i dodatkowych skryptów

Mimo że rozszerzenia AJAX Microsoft ASP.NET 2,0 obejmują cały kod skryptu w wersjach Debug i Release jako zasoby osadzone w przywoływanych zestawach, deweloperzy mogą przekierowywać elementy ScriptManager do dostosowanych plików skryptów, a także rejestrować Dodatkowe niezbędne skrypty.

Aby zastąpić domyślne powiązanie dla najczęściej dołączanych skryptów (takich jak te, które obsługują przestrzeń nazw sys. WebForms i niestandardowy system pisania), można zarejestrować dla zdarzenia `ResolveScriptReference` klasy ScriptManager. Gdy ta metoda jest wywoływana, program obsługi zdarzeń ma możliwość zmiany ścieżki do pliku skryptu. Menedżer skryptów wyśle następnie do klienta inną lub dostosowaną kopię skryptów.

Ponadto odwołania do skryptów (reprezentowane przez klasę `ScriptReference`) mogą być uwzględniane programowo lub za pomocą znaczników. W tym celu można programowo zmodyfikować kolekcję `ScriptManager.Scripts` lub uwzględnić Tagi `<asp:ScriptReference>` pod tagiem `<Scripts>`, który jest elementem podrzędnym pierwszego poziomu formantu ScriptManager.

## <a name="custom-error-handling-for-updatepanels"></a>Niestandardowa obsługa błędów dla elementu UpdatePanel

Mimo że aktualizacje są obsługiwane przez wyzwalacze określone przez kontrolki UpdatePanel, obsługa błędów i niestandardowych komunikatów o błędach jest obsługiwana przez wystąpienie formantu ScriptManager strony. Jest to realizowane przez ujawnienie zdarzenia, `AsyncPostBackError`do strony, która może następnie zapewnić niestandardową logikę obsługi wyjątków.

Korzystając ze zdarzenia AsyncPostBackError, można określić właściwość `AsyncPostBackErrorMessage`, która spowoduje wygenerowanie okna alertu po zakończeniu wywołania zwrotnego.

Możliwe jest również dostosowanie po stronie klienta zamiast korzystania z domyślnego pola alertu. na przykład może być konieczne wyświetlenie dostosowanego elementu `<div>`, a nie domyślnego okna dialogowego modalnego przeglądarki. W takim przypadku można obsłużyć błąd w skrypcie klienta:

**Lista 5: skrypt po stronie klienta do wyświetlania błędów niestandardowych**

[!code-html[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample4.html)]

Całkiem po prostu Powyższy skrypt rejestruje wywołanie zwrotne za pomocą środowiska uruchomieniowego AJAX po stronie klienta dla momentu ukończenia żądania asynchronicznego. Następnie sprawdza, czy zgłoszono błąd, a jeśli tak, przetwarza szczegóły, a wreszcie wskazuje środowisko uruchomieniowe, że błąd został obsłużony w skrypcie niestandardowym.

## <a name="globalization-and-localization-support"></a>Obsługa globalizacji i lokalizacji

Formant ScriptManager zawiera rozbudowaną obsługę lokalizowania ciągów skryptów i składników interfejsu użytkownika. Jednak ten temat wykracza poza zakres tego dokumentu. Aby uzyskać więcej informacji, zobacz oficjalny dokument dotyczący obsługi globalizacji w rozszerzeniach ASP.NET AJAX.

## <a name="the-updatepanel-control"></a>Formant UpdatePanel

## <a name="updatepanel-control-reference"></a>Formant UpdatePanel — odwołanie

Właściwości z włączoną obsługą znaczników:

| **Nazwa właściwości** | **Typ** | **Opis** |
| --- | --- | --- |
| ChildrenAsTriggers | bool | Określa, czy kontrolki potomne automatycznie wywołują odświeżanie przy ogłaszaniu zwrotnym. |
| RenderMode | Wyliczenie (blok, wbudowany) | Określa sposób wizualizacji zawartości. |
| UpdateMode | enum (zawsze, warunkowo) | Określa, czy element UpdatePanel jest zawsze odświeżany podczas częściowego renderowania lub czy jest odświeżany tylko wtedy, gdy wyzwalacz został trafiony. |

Właściwości tylko do kodu:

| **Nazwa właściwości** | **Typ** | **Opis** |
| --- | --- | --- |
| IsInPartialRendering | bool | Pobiera czy element UpdatePanel obsługuje renderowanie częściowe dla bieżącego żądania. |
| ContentTemplate | ITemplate | Pobiera szablon znaczników dla żądania aktualizacji. |
| ContentTemplateContainer | Kontrolka | Pobiera szablon programistyczny dla żądania aktualizacji. |
| Wyzwalacze | UpdatePanel-TriggerCollection | Pobiera listę wyzwalaczy skojarzonych z bieżącym elementem UpdatePanel. |

Metody kodu publicznego:

| **Nazwa metody** | **Typ** | **Opis** |
| --- | --- | --- |
| Update () | Pozycję | Aktualizuje określony element UpdatePanel programowo. Zezwala na żądanie serwera wyzwalające częściowe renderowanie elementu UpdatePanel niewyzwalanego w inny sposób. |

Elementy potomne znaczników:

| **Tag** | **Opis** |
| --- | --- |
| &lt;ContentTemplate&gt; | Określa adiustację, która ma być używana do renderowania wyniku częściowego renderowania. Element podrzędny &lt;ASP: UpdatePanel&gt;. |
| &lt;Wyzwalacze&gt; | Określa kolekcję *n* kontrolek skojarzonych z aktualizacją tego elementu UpdatePanel. Element podrzędny &lt;ASP: UpdatePanel&gt;. |
| &lt;ASP: AsyncPostBackTrigger&gt; | Określa wyzwalacz, który wywołuje częściowe renderowanie strony dla danego elementu UpdatePanel. Może to być formant, który jest elementem podrzędnym elementu UpdatePanel. Szczegółowe do nazwy zdarzenia.&gt;elementów podrzędnych wyzwalaczy &lt;. |
| &lt;ASP: PostBackTrigger&gt; | Określa kontrolkę, która powoduje odświeżenie całej strony. Może to być formant, który jest elementem podrzędnym elementu UpdatePanel. Szczegółowe dla obiektu. &gt;elementów podrzędnych wyzwalaczy &lt;. |

Formant `UpdatePanel` jest formantem, który ogranicza zawartość po stronie serwera, która zostanie uwzględniona w funkcji renderowania częściowego rozszerzeń AJAX. Nie ma żadnego limitu liczby formantów UpdatePanel, które mogą znajdować się na stronie i mogą być zagnieżdżane. Każdy element UpdatePanel jest izolowany, dzięki czemu każdy może działać niezależnie (w tym samym czasie może być uruchomionych dwa elementy UpdatePanel), ponieważ renderuje różne części strony niezależnie od odświeżania strony.

Kontrolka UpdatePanel przede wszystkim zajmuje się wyzwalaczami kontroli — Domyślnie każda kontrolka zawarta w `ContentTemplate` elementu UpdatePanel, która tworzy ogłaszanie zwrotne, jest rejestrowana jako wyzwalacz dla elementu UpdatePanel. Oznacza to, że element UpdatePanel może współpracował z domyślnymi kontrolkami związanymi z danymi (takimi jak GridView), z kontrolkami użytkownika i może być zaprogramowany w skrypcie.

Domyślnie, gdy wyzwalane jest częściowe renderowanie strony, wszystkie kontrolki UpdatePanel na stronie zostaną odświeżone, niezależnie od tego, czy kontrolki UpdatePanel zostały zdefiniowane dla tej akcji. Na przykład jeśli jeden element UpdatePanel definiuje kontrolkę Button i zostanie kliknięty formant Button, wszystkie kontrolki UpdatePanel na tej stronie będą domyślnie odświeżane. Jest to spowodowane tym, że domyślnie Właściwość `UpdateMode` elementu UpdatePanel ma wartość `Always`. Alternatywnie można ustawić właściwość UpdateMode na `Conditional`, co oznacza, że element UpdatePanel będzie odświeżany tylko po trafieniu określonego wyzwalacza.

## <a name="custom-control-notes"></a>Uwagi dotyczące kontrolek niestandardowych

Element UpdatePanel można dodać do dowolnej kontrolki użytkownika lub kontrolki niestandardowej; jednak strona, na której znajdują się te formanty, musi również zawierać kontrolkę ScriptManager z właściwością EnablePartialRendering ustawioną na **wartość true**.

Jednym ze sposobów, w którym można to uwzględnić podczas korzystania z niestandardowych formantów sieci Web, jest przesłonięcie chronionej `CreateChildControls()` metody klasy `CompositeControl`. Dzięki temu można wstrzyknąć element UpdatePanel między elementami podrzędnymi kontrolki i zewnętrznym, jeśli określisz, że strona obsługuje renderowanie częściowe; w przeciwnym razie można po prostu przydzielyć warstwę formantów podrzędnych do wystąpienia `Control` kontenera.

## <a name="updatepanel-considerations"></a>Zagadnienia dotyczące elementu UpdatePanel

Element UpdatePanel działa jako coś z czerni, zawijając ASP.NET ogłaszania zwrotnego w kontekście języka JavaScript XMLHttpRequest. Istnieją jednak istotne zagadnienia dotyczące wydajności, które należy wziąć pod uwagę, zarówno w kontekście zachowania, jak i szybkości. Aby zrozumieć, jak działa element UpdatePanel, aby móc najlepiej decydować, kiedy jego użycie jest odpowiednie, należy sprawdzić program AJAX Exchange. W poniższym przykładzie zastosowano istniejącą witrynę i program Mozilla Firefox z rozszerzeniem Firebug (Firebug przechwytuje dane XMLHttpRequest).

Rozważmy formularz, który, między innymi, ma pole tekstowe kodu pocztowego, na którym należy wypełnić pole Miasto i Województwo w formularzu lub formancie. Ten formularz ostatecznie gromadzi informacje o członkostwie, w tym nazwę użytkownika, adres i informacje kontaktowe. Istnieje wiele zagadnień projektowych, które należy wziąć pod uwagę w zależności od wymagań określonego projektu.

[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image11.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image10.png)

([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](understanding-partial-page-updates-with-asp-net-ajax/_static/image12.png))

[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image14.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image13.png)

([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](understanding-partial-page-updates-with-asp-net-ajax/_static/image15.png))

W oryginalnej iteracji tej aplikacji został utworzony formant, który zawiera całości danych rejestracyjnych użytkownika, w tym kod pocztowy, miasto i Województwo. Cały formant został opakowany w elemencie UpdatePanel i upuszczany na formularz sieci Web. Kiedy kod pocztowy jest wprowadzany przez użytkownika, element UpdatePanel wykrywa zdarzenie (odpowiednie textchangd zdarzenia w zapleczu, określając wyzwalacze lub używając właściwości ChildrenAsTriggers ustawionej na wartość true). AJAX zapisuje wszystkie pola w elemencie UpdatePanel jako przechwycone przez FireBug (zobacz diagram po prawej stronie).

Ponieważ przechwytywanie ekranu wskazuje, wartości z każdej kontrolki w elemencie UpdatePanel są dostarczane (w tym przypadku są to wszystkie puste), a także pola ViewState. Wszystkie dane powiadamiane przekraczają 9kb danych, gdy tylko pięć bajtów danych wymagało tego konkretnego żądania. Odpowiedź jest jeszcze większa bloated: łącznie, 57kb jest wysyłana do klienta, po prostu do zaktualizowania pola tekstowego i pola listy rozwijanej.

Może być również przydatne, aby zobaczyć, jak ASP.NET AJAX aktualizuje prezentację. Część odpowiedzi żądania aktualizacji elementu UpdatePanel jest wyświetlana w konsoli Firebug po lewej stronie; jest to specjalnie sformułowany ciąg rozdzielany potoku, który jest podzielony przez skrypt klienta, a następnie ponownie składany na stronie. W odniesieniu do ASP.NET AJAX ustawia właściwość *InnerHtml* elementu HTML na kliencie, który reprezentuje element UpdatePanel. Po ponownym wygenerowaniu modelu DOM przez przeglądarkę istnieje niewielkie opóźnienie, w zależności od ilości informacji, które należy przetworzyć.

Regeneracja modelu DOM wyzwala kilka dodatkowych problemów:

[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image17.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image16.png)

([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](understanding-partial-page-updates-with-asp-net-ajax/_static/image18.png))

- Jeśli skoncentrowany element HTML znajduje się w elemencie UpdatePanel, utraci fokus. W przypadku użytkowników, którzy naciśnieli klawisz Tab w celu opuszczenia pola tekstowego kod pocztowy, ich następne miejsce docelowe byłyby polem tekstowym miasto. Jednak gdy element UpdatePanel odświeżył ekran, formularz nie będzie już miał fokus, a naciśnięcie klawisza Tab spowoduje rozpoczęcie wyróżniania elementów fokusu (takich jak linki).
- Jeśli dowolny typ niestandardowego skryptu po stronie klienta jest w użyciu, który uzyskuje dostęp do elementów DOM, odwołania utrwalane przez funkcje mogą stać się unieczynnione po częściowego ogłaszania zwrotnego.

Narzędzia UpdatePanel nie są przeznaczone do przechwytywania wszystkich rozwiązań. Zamiast tego udostępniają one szybkie rozwiązanie w niektórych sytuacjach, w tym tworzenie prototypów, aktualizacje małych formantów i zapewniają znany interfejs ASP.NET deweloperom, którzy mogą znać model obiektów .NET, ale mniej — w tym modelu DOM. Istnieje wiele wariantów, które mogą spowodować lepszą wydajność, w zależności od scenariusza aplikacji:

- Rozważ użycie PageMethods i JSON (JavaScript Object Notation) umożliwia deweloperowi wywoływanie metod statycznych na stronie tak, jakby wywołanie usługi sieci Web było wywoływane. Ponieważ metody są statyczne, żaden stan nie jest konieczny; Obiekt wywołujący skrypt dostarcza parametry, a wynik jest zwracany asynchronicznie.
- Rozważ użycie usługi sieci Web i kodu JSON, jeśli jeden formant ma być używany w kilku miejscach w aplikacji. Ta ponowna część wymaga bardzo małego nakładu pracy i działa asynchronicznie.

Dołączanie funkcji za poorednictwem usług sieci Web lub metod stron ma także wady. Najpierw deweloperzy ASP.NET zazwyczaj tworzą małe składniki funkcji w kontrolkach użytkownika (pliki. ascx). Metody strony nie mogą być hostowane w tych plikach; muszą one być hostowane w obrębie rzeczywistej klasy strony. aspx. Usługi sieci Web, podobnie, muszą być hostowane w obrębie klasy. asmx. W zależności od aplikacji ta architektura może naruszać jedną regułę odpowiedzialności, w tym, że funkcjonalność pojedynczego składnika jest teraz rozłożona na dwa lub więcej elementów fizycznych, które mogą mieć niewielki lub niespójne więzi.

Na koniec Jeśli aplikacja wymaga zastosowania elementu UpdatePanel, następujące wytyczne powinny pomóc w rozwiązywaniu problemów i konserwacji.

- **Zagnieżdżaj element UpdatePanel jak najmniejszej liczby, a nie tylko w jednostkach, ale również w różnych jednostkach kodu.** Na przykład posiadanie elementu UpdatePanel na stronie, która otacza formant, chociaż ta kontrolka zawiera również element UpdatePanel, który zawiera inną kontrolkę zawierającą element UpdatePanel, to zagnieżdżenie między jednostkami. Pomaga to w czyszczeniu, które elementy powinny być odświeżane, i uniemożliwia nieoczekiwane odświeżenie podrzędnych elementów UpdatePanel.
- **Pozostaw Właściwość *ChildrenAsTriggers* ustawioną na wartość false i jawnie ustaw Wyzwalanie zdarzeń.** Korzystanie z kolekcji `<Triggers>` jest znacznie bardziej wyraźniejsze w obsłudze zdarzeń i może zapobiec nieoczekiwanemu zachowaniu, pomagając z zadaniami konserwacji i wymuszać, że deweloper może zadecydować o zdarzeniu.
- **Użyj najmniejszej możliwej jednostki do osiągnięcia funkcjonalności.** Jak wskazano w omówieniu usługi kodów pocztowych, należy zawijać tylko minimalny czas do serwera, łączne przetwarzanie i wydajność wymiany klient-serwer.

## <a name="the-updateprogress-control"></a>Kontrolka UpdateProgress

## <a name="updateprogress-control-reference"></a>UpdateProgress — informacje o formancie

Właściwości z włączoną obsługą znaczników:

| **Nazwa właściwości** | **Typ** | **Opis** |
| --- | --- | --- |
| AssociatedUpdate-PanelID | Ciąg | Określa identyfikator elementu UpdatePanel, który ma być zgłaszany przez ten UpdateProgress. |
| DisplayAfter | int | Określa limit czasu (w milisekundach), po którym zostanie wyświetlony ten formant po rozpoczęciu żądania asynchronicznego. |
| DynamicLayout | bool | Określa, czy postęp jest renderowany dynamicznie. |

Elementy potomne znaczników:

| **Tag** | **Opis** |
| --- | --- |
| &lt;element ProgressTemplate&gt; | Zawiera zestaw szablonów formantów dla zawartości, która będzie wyświetlana z tą kontrolką. |

Kontrolka UpdateProgress udostępnia miarę informacji zwrotnych, aby zapewnić zainteresowanie użytkownikami podczas wykonywania niezbędnych zadań do transportu na serwer. Dzięki temu użytkownicy mogą wiedzieć, że wykonujesz coś, mimo że może nie być oczywisty, szczególnie dlatego, że większość użytkowników jest używana do odświeżania strony i oglądania wyróżnionego paska stanu.

Jako notatka, formanty UpdateProgress mogą znajdować się w dowolnym miejscu w hierarchii strony. Jednak w przypadkach, w których częściowe ogłaszanie zwrotne jest inicjowane z podrzędnego elementu UpdatePanel (gdzie element UpdatePanel jest zagnieżdżony w innym elemencie UpdatePanel), ogłaszania zwrotne wyzwalające podrzędne elementy UpdatePanel spowodują wyświetlenie szablonów UpdateProgress dla elementu podrzędnego UpdatePanel i nadrzędny element UpdatePanel. Ale jeśli wyzwalacz jest bezpośrednim elementem podrzędnym nadrzędnego elementu UpdatePanel, zostaną wyświetlone tylko szablony UpdateProgress skojarzone z elementem nadrzędnym.

## <a name="summary"></a>Podsumowanie

Rozszerzenia AJAX Microsoft ASP.NET to zaawansowane produkty, które ułatwiają zwiększenie dostępności zawartości sieci Web i zapewniają bogatsze środowisko użytkownika aplikacji sieci Web. Jako część rozszerzeń ASP.NET AJAX, kontrolki renderowania stron częściowych, w tym kontrolki ScriptManager, UpdatePanel i UpdateProgress, są najbardziej widocznymi składnikami zestawu narzędzi.

Składnik ScriptManager integruje udostępnianie kodu JavaScript klienta dla rozszerzeń, a także udostępnia różne składniki serwera i klienta, które współpracują ze sobą przy minimalnych inwestycjach programistycznych.

Formant UpdatePanel jest widocznym magiczną ramką w elemencie UpdatePanel może być CodeBehind po stronie serwera i nie wyzwalać odświeżania strony. Kontrolki UpdatePanel mogą być zagnieżdżane i mogą być zależne od kontrolek w innych elementach UpdatePanel. Domyślnie elementy UpdatePanel obsługują wszystkie ogłaszanie zwrotne wywoływane przez ich elementy podrzędne, chociaż ta funkcja może być dostrojona lub programowo.

W przypadku korzystania z formantu UpdatePanel deweloperzy powinni mieć świadomość wpływu na wydajność, który mógłby wystąpić. Potencjalne alternatywy obejmują usługi sieci Web i metody stron, chociaż należy rozważyć projekt aplikacji.

Formant UpdateProgress umożliwia użytkownikowi uzyskanie informacji o tym, że lub nie jest ignorowany, oraz że żądanie w tle jest wykonywane, gdy strona nie wykonuje żadnych czynności w celu reagowania na dane wejściowe użytkownika. Umożliwia także przerwanie efektów renderowania częściowego.

Wspólnie te narzędzia ułatwiają tworzenie rozbudowanego i bezproblemowego środowiska użytkownika, dzięki czemu serwer działa mniej widoczny dla użytkownika i przerywa przepływ pracy.

## <a name="bio"></a>Materiał

Scott Cate pracował z technologiami sieci Web firmy Microsoft od 1997 i jest prezydentem myKB.com ([www.myKB.com](http://www.myKB.com)), w którym wyspecjalizowany jest pisanie aplikacji opartych na ASP.NET, które są zgodne z podstawowymi rozwiązaniami oprogramowania. W witrynie Scotta można skontaktować się z pocztą e-mail na [scott.cate@myKB.com](mailto:scott.cate@myKB.com) lub w blogu w witrynie [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Dalej](understanding-asp-net-ajax-updatepanel-triggers.md)

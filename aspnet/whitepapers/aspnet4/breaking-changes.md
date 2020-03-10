---
uid: whitepapers/aspnet4/breaking-changes
title: ASP.NET 4 istotne zmiany | Microsoft Docs
author: rick-anderson
description: Ten dokument zawiera informacje o zmianach wprowadzonych dla wersji programu .NET Framework w wersji 4, które mogą mieć wpływ na aplikacje, które zostały utworzone za pomocą...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: d601c540-f86b-4feb-890c-20c806b3da6c
msc.legacyurl: /whitepapers/aspnet4/breaking-changes
msc.type: content
ms.openlocfilehash: 8ccad3b40a723c92a3164de082e1f94577141008
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78546250"
---
# <a name="aspnet-4-breaking-changes"></a>Zmiany powodujące niezgodność w platformie ASP.NET 4

> Ten dokument zawiera informacje o zmianach wprowadzonych dla wersji programu .NET Framework w wersji 4, które mogą mieć wpływ na aplikacje, które zostały utworzone w starszych wersjach, w tym wydania ASP.NET 4 beta 1 i beta 2.
> 
> [Pobierz ten oficjalny dokument](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_Breaking_Changes.pdf)

<a id="0.1__Toc256768952"></a><a id="0.1__Toc256770056"></a>

## <a name="contents"></a>Spis treści

[Ustawienie ControlRenderingCompatibilityVersion w pliku Web. config](#0.1__Toc256770141 "_Toc256770141")  
[ClientIDMode zmiany](#0.1__Toc256770142 "_Toc256770142")  
[HtmlEncode i UrlEncode teraz kodują znaki pojedynczego cudzysłowu](#0.1__Toc256770143 "_Toc256770143")  
[Analizator strony ASP.NET (. aspx) jest bardziej restrykcyjny](#0.1__Toc256770144 "_Toc256770144")  
[Zaktualizowano pliki definicji przeglądarki](#0.1__Toc256770145 "_Toc256770145")  
[System. Web. Mobile. dll został usunięty z głównego pliku konfiguracji sieci Web](#0.1__Toc256770146 "_Toc256770146")  
[Weryfikacja żądania ASP.NET](#0.1__Toc256770147 "_Toc256770147")  
[Domyślny algorytm wyznaczania wartości skrótu jest teraz HMACSHA256](#0.1__Toc256770148 "_Toc256770148")  
[Błędy konfiguracji związane z nową konfiguracją główną ASP.NET 4](#0.1__Toc256770149 "_Toc256770149")  
[Nie można uruchomić aplikacji podrzędnych ASP.NET 4, jeśli są one w aplikacjach ASP.NET 2,0 lub ASP.NET 3,5](#0.1__Toc256770150 "_Toc256770150")  
[Nie można uruchomić witryn sieci Web ASP.NET 4 na komputerach, na których zainstalowano program SharePoint](#0.1__Toc256770151 "_Toc256770151")  
[Właściwość HttpRequest. FilePath nie zawiera już wartości PathInfo](#0.1__Toc256770152 "_Toc256770152")  
[Aplikacje ASP.NET 2,0 mogą generować błędy HttpException odwołujące się do EURL. axd](#0.1__Toc256770153 "_Toc256770153")  
[Programy obsługi zdarzeń mogą nie zostać zgłoszone w dokumencie domyślnym w trybie zintegrowanym usług IIS 7 lub IIS 7,5](#0.1__Toc256770154 "_Toc256770154")  
[Zmiany implementacji zabezpieczenia dostępu kodu ASP.NET (CAS)](#0.1__Toc256770155 "_Toc256770155")  
[MembershipUser i inne typy w przestrzeni nazw System. Web. Security zostały przeniesione](#0.1__Toc256770156 "_Toc256770156")  
[Buforowanie zmian w danych wyjściowych w różnych \* nagłówku HTTP](#0.1__Toc256770157 "_Toc256770157")  
[Typy system. Web. Security dotyczące usługi Passport są przestarzałe](#0.1__Toc256770158 "_Toc256770158")  
[Właściwość MenuItem. PopOutImageUrl nie może renderować obrazu w ASP.NET 4](#0.1__Toc256770159 "_Toc256770159")  
[Menu. StaticPopOutImageUrl i menu. DynamicPopOutImageUrl nie renderuje obrazów, gdy ścieżki zawierają ukośniki odwrotne](#0.1__Toc256770160 "_Toc256770160")  
[Zastrzeżenie](#0.1__Toc256770161 "_Toc256770161")

<a id="0.1__ControlRenderingCompatibilityVersio"></a><a id="0.1__Toc245724853"></a><a id="0.1__Toc255587630"></a><a id="0.1__Toc256770141"></a>

## <a name="controlrenderingcompatibilityversion-setting-in-the-webconfig-file"></a>Ustawienie ControlRenderingCompatibilityVersion w pliku Web. config

Kontrolki ASP.NET zostały zmodyfikowane w .NET Framework w wersji 4, aby umożliwić dokładniejsze określenie sposobu renderowania znaczników. W poprzednich wersjach .NET Framework niektóre kontrolki emitują znaczniki, których nie można wyłączyć. Domyślnie ASP.NET 4 Ten typ znacznika nie jest już generowany.

Jeśli używasz programu Visual Studio 2010 do uaktualnienia aplikacji z ASP.NET 2,0 lub ASP.NET 3,5, narzędzie automatycznie dodaje ustawienie do pliku `Web.config`, który zachowuje starsze renderowanie. Jednak w przypadku uaktualniania aplikacji przez zmianę puli aplikacji w programie IIS na wartość docelową .NET Framework 4, ASP.NET domyślnie korzysta z nowego trybu renderowania. Aby wyłączyć nowy tryb renderowania, Dodaj następujące ustawienie w pliku `Web.config`:

[!code-xml[Main](breaking-changes/samples/sample1.xml)]

Renderowanie główne zmienia się w następujący sposób:

- Formanty **Image** i **kliknięto element ImageButton** nie są już renderowane atrybut `border="0"`.
- Klasy **BaseValidator** i kontrolki walidacji, które pochodzą od niej, nie są już domyślnie renderowane czerwony tekst.
- Formant **parametr HtmlForm** nie renderuje atrybutu **name** .
- Kontrolka **tabeli** nie jest już renderowana `border="0"` atrybutu.
- Kontrolki, które nie są przeznaczone do wprowadzania danych przez użytkownika (na przykład kontrolka **etykieta** ), nie są już renderowane atrybut `disabled="disabled"`, jeśli ich Właściwość **Enabled** ma **wartość false** (lub jeśli dziedziczy to ustawienie z kontrolki kontenera).

<a id="0.1__Toc245724854"></a><a id="0.1__Toc255587631"></a><a id="0.1__Toc256770142"></a>

## <a name="clientidmode-changes"></a>ClientIDMode zmiany

Ustawienie **ClientIDMode** w ASP.NET 4 pozwala określić, jak ASP.NET generuje atrybut **ID** dla elementów HTML. W poprzednich wersjach ASP.NET zachowanie domyślne było równoważne ustawieniu **AutoID** **ClientIDMode**. Jednak ustawienie domyślne jest teraz **przewidywalne**.

Jeśli używasz programu Visual Studio 2010 do uaktualnienia aplikacji z ASP.NET 2,0 lub ASP.NET 3,5, narzędzie automatycznie dodaje ustawienie do pliku `Web.config`, który zachowuje zachowanie wcześniejszych wersji .NET Framework. Jednak w przypadku uaktualniania aplikacji przez zmianę puli aplikacji w programie IIS na wartość docelową .NET Framework 4, ASP.NET domyślnie używa trybu nowy. Aby wyłączyć nowy tryb identyfikatora klienta, należy dodać następujące ustawienie w pliku `Web.config`:

[!code-xml[Main](breaking-changes/samples/sample2.xml)]

<a id="0.1__Toc245724855"></a><a id="0.1__Toc255587632"></a><a id="0.1__Toc256770143"></a>

## <a name="htmlencode-and-urlencode-now-encode-single-quotation-marks"></a>HtmlEncode i UrlEncode teraz kodują znaki pojedynczego cudzysłowu

W ASP.NET 4 metody **HtmlEncode** i **UrlEncode** klas **HttpUtility** i **HttpServerUtility** zostały zaktualizowane, aby kodować znak pojedynczego cudzysłowu (') w następujący sposób:

- Metoda **HtmlEncode** koduje wystąpienia pojedynczego cudzysłowu jako '.
- Metoda **UrlEncode** koduje wystąpienia pojedynczego cudzysłowu jako %27.

<a id="0.1__Toc255587633"></a><a id="0.1__Toc256770144"></a><a id="0.1__Toc245724856"></a>

## <a name="aspnet-page-aspx-parser-is-stricter"></a>Analizator strony ASP.NET (. aspx) jest bardziej restrykcyjny

Analizator strony dla stron ASP.NET (pliki`.aspx`) i kontrolki użytkownika (pliki`.ascx`) są bardziej rygorystyczne w ASP.NET 4 i odrzucają więcej wystąpień nieprawidłowych znaczników. Na przykład następujące dwa fragmenty kodu zostaną pomyślnie przeanalizowane we wcześniejszych wersjach ASP.NET, ale teraz spowodują błędy analizatora w ASP.NET 4.

[!code-aspx[Main](breaking-changes/samples/sample3.aspx)]

Zwróć uwagę na nieprawidłowy średnik na końcu tagu **HiddenField** .

[!code-aspx[Main](breaking-changes/samples/sample4.aspx)]

Zwróć uwagę na atrybut niezamkniętego **stylu** , który jest uruchamiany w atrybucie **CssClass** .

<a id="0.1__Toc255587634"></a><a id="0.1__Toc256770145"></a>

## <a name="browser-definition-files-updated"></a>Zaktualizowano pliki definicji przeglądarki

Pliki definicji przeglądarki zostały zaktualizowane w celu uwzględnienia informacji o nowych i zaktualizowanych przeglądarkach i urządzeniach. Zostały usunięte starsze przeglądarki i urządzenia, takie jak Netscape Navigator, i dodano nowsze przeglądarki i urządzenia, takie jak Google Chrome i Apple iPhone.

Jeśli aplikacja zawiera niestandardowe definicje przeglądarki dziedziczące z jednej z definicji przeglądarki, które zostały usunięte, zostanie wyświetlony komunikat o błędzie. Na przykład, jeśli folder `App_Browsers` zawiera definicję przeglądarki, która dziedziczy z definicji przeglądarki IE2, zostanie wyświetlony następujący komunikat o błędzie:

- Nie można znaleźć elementu przeglądarki lub bramy o IDENTYFIKATORze "IE2".

> [!NOTE]
> Obiekt **HttpBrowserCapabilities** (udostępniany przez właściwość **żądanie** strony) jest oparty na plikach definicji przeglądarki. W związku z tym informacje zwracane przez uzyskanie dostępu do właściwości tego obiektu w ASP.NET 4 mogą różnić się od informacji zwracanych we wcześniejszej wersji ASP.NET.

Możesz powrócić do starych plików definicji przeglądarki, kopiując pliki definicji przeglądarki z następującego folderu:

[!code-console[Main](breaking-changes/samples/sample5.cmd)]

Skopiuj pliki do odpowiedniego folderu `\CONFIG\Browsers` dla ASP.NET 4. Po skopiowaniu plików uruchom narzędzie wiersza polecenia ASPNET\_RegBrowsers. exe.

<a id="0.1__Toc255587635"></a><a id="0.1__Toc256770146"></a>

## <a name="systemwebmobiledll-removed-from-root-web-configuration-file"></a>System. Web. Mobile. dll został usunięty z głównego pliku konfiguracji sieci Web

W poprzednich wersjach programu ASP.NET odwołanie do zestawu System. Web. Mobile. dll zostało dołączone do głównego pliku `Web.config` w sekcji **zestawy** w obszarze. Aby zwiększyć wydajność, odwołanie do tego zestawu zostało usunięte.

Zestaw system. Web. Mobile. dll jest zawarty w ASP.NET 4, ale jest przestarzały. Jeśli chcesz użyć typów z zestawu System. Web. Mobile. dll, Dodaj odwołanie do tego zestawu do głównego pliku `Web.config` lub do pliku `Web.config` aplikacji. Na przykład jeśli chcesz użyć dowolnego z (przestarzałe) kontrolek ASP.NET Mobile, musisz dodać odwołanie do zestawu System. Web. Mobile. dll do pliku `Web.config`.

<a id="0.1__Toc245724857"></a><a id="0.1__Toc255587636"></a><a id="0.1__Toc256770147"></a>

## <a name="aspnet-request-validation"></a>Weryfikacja żądania ASP.NET

Funkcja walidacji żądania w programie ASP.NET zapewnia określony poziom ochrony domyślnej przed atakami na skrypty między lokacjami (XSS). W poprzednich wersjach programu ASP.NET sprawdzanie poprawności żądania zostało włączone domyślnie. Jednak ma to zastosowanie tylko do stron ASP.NET (plików`.aspx` i ich plików klas) i tylko wtedy, gdy te strony były wykonywane.

W ASP.NET 4 domyślnie sprawdzanie poprawności żądania jest włączone dla wszystkich żądań, ponieważ jest włączone przed fazą **BeginRequest** żądania HTTP. W związku z tym sprawdzanie poprawności żądania dotyczy żądań wszystkich zasobów ASP.NET, a nie tylko żądań stron. aspx. Obejmuje to żądania, takie jak wywołania usługi sieci Web i niestandardowe programy obsługi protokołu HTTP. Sprawdzanie poprawności żądania jest również aktywne, gdy niestandardowe moduły HTTP odczytuje zawartość żądania HTTP.

W związku z tym błędy walidacji żądania mogą teraz wystąpić w przypadku żądań, które wcześniej nie powodowały błędów. Aby przywrócić zachowanie funkcji walidacji żądania ASP.NET 2,0, Dodaj następujące ustawienie w pliku `Web.config`:

[!code-xml[Main](breaking-changes/samples/sample6.xml)]

Jednak zalecamy przeanalizowanie wszelkich błędów weryfikacji żądań w celu ustalenia, czy istniejące programy obsługi, moduły lub inny kod niestandardowy uzyskują dostęp do potencjalnie niebezpiecznych danych wejściowych HTTP, które mogą być nosicielami ataków typu XSS.

<a id="0.1__Toc245724858"></a><a id="0.1__Toc255587637"></a><a id="0.1__Toc256770148"></a>

## <a name="default-hashing-algorithm-is-now-hmacsha256"></a>Domyślny algorytm wyznaczania wartości skrótu jest teraz HMACSHA256

ASP.NET używa algorytmów szyfrowania i wyznaczania wartości skrótu, aby zabezpieczyć dane, takie jak pliki cookie uwierzytelniania formularzy i stan widoku. Domyślnie ASP.NET 4 używa teraz algorytmu HMACSHA256 dla operacji mieszania dla plików cookie i stanu widoku. We wcześniejszych wersjach ASP.NET użyto starszego algorytmu HMACSHA1.

W przypadku korzystania z aplikacji mieszanych ASP.NET 2.0/ASP. NET 4 mogą być narażone Twoje aplikacje, w przypadku których dane takie jak pliki cookie uwierzytelniania formularzy muszą działać w wersjach across.NET Framework. Aby skonfigurować aplikację sieci Web ASP.NET 4 do korzystania ze starszego algorytmu HMACSHA1, Dodaj następujące ustawienie w pliku `Web.config`:

[!code-xml[Main](breaking-changes/samples/sample7.xml)]

<a id="0.1__Toc245724859"></a><a id="0.1__Toc255587638"></a><a id="0.1__Toc256770149"></a>

## <a name="configuration-errors-related-to-new-aspnet-4-root-configuration"></a>Błędy konfiguracji związane z nową konfiguracją główną ASP.NET 4

Pliki konfiguracji głównej (plik `machine.config` i główny plik `Web.config`) dla .NET Framework 4 (i w związku z tym ASP.NET 4) zostały zaktualizowane w celu uwzględnienia większości standardowych informacji o konfiguracji, które w ASP.NET 3,5 zostały znalezione w plikach `Web.config` aplikacji. Ze względu na złożoność zarządzanych systemów konfiguracyjnych usług IIS 7 i IIS 7,5, uruchamianie aplikacji ASP.NET 3,5 w ASP.NET 4 i usług IIS 7 i IIS 7,5 może spowodować błędy konfiguracji ASP.NET lub usług IIS.

Zalecamy uaktualnienie aplikacji ASP.NET 3,5 do ASP.NET 4 przy użyciu narzędzi uaktualniania projektu w programie Visual Studio 2010, jeśli jest to praktycznie możliwe. Program Visual Studio 2010 automatycznie modyfikuje plik `Web.config` aplikacji ASP.NET 3,5, aby zawierał odpowiednie ustawienia dla ASP.NET 4.

Jednak jest to obsługiwany scenariusz do uruchamiania aplikacji ASP.NET 3,5 przy użyciu .NET Framework 4 bez ponownej kompilacji. W takim przypadku może być konieczne ręczne zmodyfikowanie pliku `Web.config` aplikacji przed uruchomieniem aplikacji w .NET Framework 4 i w usługach IIS 7 lub IIS 7,5.

W następnych dwóch sekcjach opisano zmiany, które mogą być konieczne w przypadku różnych kombinacji oprogramowania.

**Windows Vista z dodatkiem SP1 lub Windows Server 2008 z dodatkiem SP1, gdzie nie są zainstalowane żadne poprawki KB958854 ani SP2.** W tej konfiguracji system konfiguracji usług IIS 7 niepoprawnie Scala zarządzaną aplikację, porównując plik `Web.config` na poziomie aplikacji z plikami `machine.config` ASP.NET 2,0. Z tego powodu pliki `Web.config` na poziomie aplikacji z .NET Framework 3,5 lub nowszych muszą mieć definicję sekcji konfiguracji **System. Web. Extensions** (element), aby nie powodowała błędu sprawdzania poprawności usług IIS 7.

Jednak ręcznie zmodyfikowane wpisy plików `Web.config` na poziomie aplikacji, które nie pasują dokładnie do oryginalnych definicji sekcji konfiguracji, które zostały wprowadzone w programie Visual Studio 2008, spowodują błędy konfiguracji ASP.NET. (Domyślne wpisy konfiguracji, które są generowane przez program Visual Studio 2008, działają poprawnie). Typowy problem polega na tym, że ręcznie zmodyfikowane `Web.config` pliki pozostawiają atrybuty konfiguracji **AllowDefinition** i **requirePermission** , które znajdują się w różnych definicjach sekcji konfiguracji. Powoduje to niezgodność między skróconą sekcją konfiguracji w plikach `Web.config` na poziomie aplikacji a kompletną definicją w pliku `machine.config` ASP.NET 4. W związku z tym w czasie wykonywania system konfiguracji ASP.NET 4 zgłasza błąd konfiguracji.

**Windows Vista z dodatkiem SP2, Windows Server 2008 z dodatkiem SP2, Windows 7, Windows Server 2008 R2 oraz Windows Vista z dodatkiem SP1 i Windows Server 2008 SP1, w których zainstalowano poprawkę KB958854.**

W tym scenariuszu system konfiguracji natywnych usług IIS 7 i IIS 7,5 zwraca błąd konfiguracji, ponieważ wykonuje porównanie tekstu dla atrybutu **typu** zdefiniowanego dla programu obsługi sekcji konfiguracji zarządzanej. Ponieważ wszystkie pliki `Web.config`, które są generowane przez program Visual Studio 2008 i Visual Studio 2008 z dodatkiem SP1, mają "3,5" w ciągu typu dla systemu. programy obsługi sekcji konfiguracyjnych **Web. Extensions** (i powiązane), a ponieważ plik ASP.NET 4 `machine.config` ma wartość "4,0" w atrybucie **typu** dla tego samego programu obsługi sekcji konfiguracji, aplikacje generowane w programie Visual Studio 2008 lub Visual Studio 2008 SP1 zawsze kończą się niepowodzeniem weryfikacji konfiguracji w usługach iis 7 i IIS 7,5.

<a id="0.1__Toc251910248"></a>

### <a name="resolving-these-issues"></a>Rozwiązywanie tych problemów

Obejście w pierwszym scenariuszu polega na zaktualizowaniu pliku `Web.config` na poziomie aplikacji przez dołączenie standardowego tekstu konfiguracji z pliku `Web.config`, który został wygenerowany automatycznie przez program Visual Studio 2008.

W przypadku pierwszego scenariusza należy zainstalować dodatek Service Pack 2 dla systemu Vista lub Windows Server 2008 na komputerze lub zainstalować poprawkę KB958854 ([https://support.microsoft.com/kb/958854](https://support.microsoft.com/kb/958854)), aby naprawić nieprawidłowe zachowanie podczas scalania konfiguracji programu IIS. Jednak po wykonaniu jednej z tych akcji aplikacja prawdopodobnie napotka błąd konfiguracji z powodu problemu opisanego w drugim scenariuszu.

Obejście drugiego scenariusza polega na usunięciu lub dodaniu komentarza do wszystkich definicji i grup sekcji konfiguracji **System. Web. Extensions** z pliku `Web.config` na poziomie aplikacji. Te definicje są zwykle w górnej części pliku `Web.config` na poziomie aplikacji i mogą być identyfikowane przez element **configSections** i jego elementy podrzędne.

W obu scenariuszach zaleca się również ręczne usunięcie sekcji **System. CodeDom** , chociaż nie jest to wymagane.

<a id="0.1__Toc252995490"></a><a id="0.1__Toc255587639"></a><a id="0.1__Toc256770150"></a><a id="0.1__Toc245724860"></a>

## <a name="aspnet-4-child-applications-fail-to-start-when-under-aspnet-20-or-aspnet-35-applications"></a>Nie można uruchomić aplikacji podrzędnych ASP.NET 4, jeśli są one w aplikacjach ASP.NET 2,0 lub ASP.NET 3,5

Nie można uruchomić aplikacji ASP.NET 4, które są skonfigurowane jako elementy podrzędne aplikacji, które działają we wcześniejszych wersjach programu ASP.NET z powodu błędów konfiguracji lub kompilacji. W poniższym przykładzie pokazano strukturę katalogów dla aplikacji, której dotyczy dana aplikacja.

`/parentwebapp` (skonfigurowany do korzystania z ASP.NET 2,0 lub ASP.NET 3,5)  
`/childwebapp` (skonfigurowany do korzystania z ASP.NET 4)

Aplikacja w folderze `childwebapp` nie zostanie uruchomiona w usługach IIS 7 lub IIS 7,5 i zgłosi błąd konfiguracji. Tekst błędu będzie zawierać komunikat podobny do następującego:

- `The requested page cannot be accessed because the related configuration data for the page is invalid.`

- `The configuration section 'configSections' cannot be read because it is missing a section declaration.`

W usługach IIS w wersji 6 nie można uruchomić aplikacji w folderze `childwebapp`, ale zostanie wyraportowany inny błąd. Na przykład tekst błędu może zawierać następujące elementy:

- `The value for the 'compilerVersion' attribute in the provider options must be 'v4.0' or later if you are compiling for version 4.0 or later of the .NET Framework. To compile this Web application for version 3.5 or earlier of the .NET Framework, remove the 'targetFramework' attribute from the element of the Web.config file`

Te scenariusze występują, ponieważ informacje o konfiguracji z aplikacji nadrzędnej w folderze `parentwebapp` są częścią hierarchii informacji o konfiguracji, która określa finalne scalone ustawienia konfiguracji, które są używane przez podrzędną aplikację sieci Web w folderze `childwebapp`. W zależności od tego, czy aplikacja sieci Web ASP.NET 4 jest uruchomiona w usługach IIS 7 (lub IIS 7,5) lub w usługach IIS 6, system konfiguracji usług IIS lub system kompilacji ASP.NET 4 zwróci błąd.

Kroki, które należy wykonać, aby rozwiązać ten problem i pobrać podrzędną aplikację ASP.NET 4, zależy od tego, czy aplikacja ASP.NET 4 działa w usługach IIS w wersji 6 lub w usługach IIS 7 (lub IIS 7,5).

### <a name="step-1-iis-7-or-iis-75-only"></a>Krok 1 (tylko usługi IIS 7 lub IIS 7,5)

Ten krok jest niezbędny tylko w systemach operacyjnych z uruchomionymi usługami IIS 7 lub IIS 7,5, które obejmują systemy Windows Vista, Windows Server 2008, Windows 7 i Windows Server 2008 R2.

Przenieś definicję **configSections** w pliku `Web.config` aplikacji nadrzędnej (aplikacji z systemem ASP.NET 2,0 lub ASP.NET 3,5) do głównego pliku `Web.config` dla the.NET Framework 2,0. Natywny system konfiguracji usług IIS 7 i IIS 7,5 skanuje element **configSections** podczas scalania hierarchii plików konfiguracji. Przeniesienie definicji **configSections** z pliku `Web.config` nadrzędnej aplikacji sieci Web do pliku głównego `Web.config` skutecznie spowoduje ukrycie elementu z procesu scalania konfiguracji, który występuje w przypadku aplikacji podrzędnej ASP.NET 4.

W 32-bitowym systemie operacyjnym lub dla puli aplikacji 32-bitowej główny plik `Web.config` dla ASP.NET 2,0 i ASP.NET 3,5 zwykle znajduje się w następującym folderze:

`C:\Windows\Microsoft.NET\Framework\v2.0.50727\CONFIG`

W 64-bitowym systemie operacyjnym lub dla puli aplikacji 64-bitowej główny plik `Web.config` dla ASP.NET 2,0 i ASP.NET 3,5 zwykle znajduje się w następującym folderze:

`C:\Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG`

Jeśli na komputerze z 64-bitowym działa zarówno 32-bitowe, jak i 64-bitowe aplikacje sieci Web, należy przenieść element **configSections** do plików głównych `Web.config` zarówno dla systemów 32-bit, jak i 64-bit.

Po umieszczeniu elementu **configSections** w pliku głównym `Web.config` wklej sekcję bezpośrednio po elemencie **konfiguracji** . Poniższy przykład pokazuje, jak powinna wyglądać Górna część pliku głównego `Web.config` po zakończeniu przesuwania elementów.

> [!NOTE]
> W poniższym przykładzie wiersze zostały opakowane w celu zapewnienia czytelności.

[!code-xml[Main](breaking-changes/samples/sample8.xml)]

### <a name="step-2-all-versions-of-iis"></a>Krok 2 (wszystkie wersje usług IIS)

Ten krok jest wymagany, jeśli podrzędna aplikacja sieci Web ASP.NET 4 jest uruchomiona w usługach IIS 6 lub IIS 7 (lub IIS 7,5).

W pliku `Web.config` nadrzędnej aplikacji sieci Web, na której działa ASP.NET 2 lub ASP.NET 3,5, Dodaj tag **lokalizacji** , który jawnie określa (zarówno w przypadku systemów konfiguracji usług IIS, jak i ASP.NET), że wpisy konfiguracji mają zastosowanie tylko do nadrzędnej aplikacji sieci Web. Poniższy przykład pokazuje składnię elementu **lokalizacji** do dodania:

[!code-xml[Main](breaking-changes/samples/sample9.xml)]

Poniższy przykład pokazuje, jak tag **Location** służy do zawijania wszystkich sekcji konfiguracyjnych zaczynających się od sekcji **AppSettings** i kończąc na **System. WebServer** .

[!code-xml[Main](breaking-changes/samples/sample10.xml)]

Po wykonaniu kroków 1 i 2 podrzędne aplikacje sieci Web ASP.NET 4 zostaną uruchomione bez błędów.

<a id="0.1__Toc252995491"></a><a id="0.1__Toc255587640"></a><a id="0.1__Toc256770151"></a>

## <a name="aspnet-4-web-sites-fail-to-start-on-computers-where-sharepoint-is-installed"></a>Nie można uruchomić witryn sieci Web ASP.NET 4 na komputerach, na których zainstalowano program SharePoint

Serwery sieci Web, na których działa program SharePoint, mają plik `Web.config`, który jest wdrażany w katalogu głównym witryny sieci Web programu SharePoint (na przykład `c:\inetpub\wwwroot\web.config` dla domyślnej witryny sieci Web). W tym pliku `Web.config` program SharePoint ustawia niestandardowy poziom zaufania częściowego o nazwie WSS\_minimalny.

Jeśli spróbujesz uruchomić witrynę sieci Web ASP.NET 4, która jest wdrażana jako element podrzędny tego typu witryny sieci Web programu SharePoint, zostanie wyświetlony następujący błąd:

`Could not find permission set named 'ASP.NET'.`

Ten błąd występuje, ponieważ infrastruktura zabezpieczeń dostępu kodu (CAS) ASP.NET 4 szuka zestawu uprawnień o nazwie ASP.NET. Jednak częściowo zaufany plik konfiguracyjny, do którego odwołuje się program WSS\_minimalny nie zawiera żadnych zestawów uprawnień o tej nazwie.

Obecnie nie ma dostępnej wersji programu SharePoint, która jest zgodna z ASP.NET. W związku z tym nie należy próbować uruchamiać witryny sieci Web ASP.NET 4 jako lokacji podrzędnej pod witrynami sieci Web programu SharePoint.

<a id="0.1__Toc255587641"></a><a id="0.1__Toc256770152"></a>

## <a name="the-httprequestfilepath-property-no-longer-includes-pathinfo-values"></a>Właściwość HttpRequest. FilePath nie zawiera już wartości PathInfo

Poprzednie wersje ASP.NET zawierają wartość **PathInfo** w wartości zwracanej z różnych właściwości związanych z ścieżką pliku, w tym **HttpRequest. FilePath**, **HttpRequest. AppRelativeCurrentExecutionFilePath**i **HttpRequest. CurrentExecutionFilePath**. ASP.NET 4 nie zawiera już wartości **PathInfo** w wartościach zwracanych z tych właściwości. Zamiast tego informacje **PathInfo** są dostępne w usłudze **HttpRequest. PathInfo**. Załóżmy na przykład następujący fragment adresu URL:

`/testapp/Action.mvc/SomeAction`

We wcześniejszych wersjach ASP.NET właściwości **HttpRequest** mają następujące wartości:

**HttpRequest. FilePath**: `/testapp/Action.mvc/SomeAction`

**HttpRequest. PathInfo**: (puste)

W ASP.NET 4 właściwości **HttpRequest** powinny mieć następujące wartości:

**HttpRequest. FilePath**: `/testapp/Action.mvc`

**HttpRequest. PathInfo**: `SomeAction`

<a id="0.1__Toc252995493"></a><a id="0.1__Toc255587642"></a><a id="0.1__Toc256770153"></a><a id="0.1__Toc245724861"></a>

## <a name="aspnet-20-applications-might-generate-httpexception-errors-that-reference-eurlaxd"></a>Aplikacje ASP.NET 2,0 mogą generować błędy HttpException odwołujące się do EURL. axd

Po włączeniu ASP.NET 4 w usługach IIS 6, ASP.NET 2,0, które działają w usługach IIS 6 (w systemie Windows Server 2003 lub Windows Server 2003 R2) mogą generować następujące błędy:

`System.Web.HttpException: Path '/[yourApplicationRoot]/eurl.axd/[Value]' was not found.`

Ten błąd występuje, ponieważ gdy ASP.NET wykryje, że witryna sieci Web jest skonfigurowana do używania ASP.NET 4, składnik macierzysty ASP.NET 4 przekazuje bezwzględny adres URL do zarządzanej części ASP.NET do dalszej obróbki. Jeśli jednak katalogi wirtualne, które znajdują się poniżej witryny sieci Web ASP.NET 4, są skonfigurowane do korzystania z ASP.NET 2,0, przetwarzanie adresu URL bezrozszerzenia w ten sposób spowoduje zmodyfikowanie adresu URL, który zawiera ciąg "EURL. axd". Ten zmodyfikowany adres URL jest następnie wysyłany do aplikacji ASP.NET 2,0. ASP.NET 2,0 nie może rozpoznać formatu "EURL. axd". W związku z tym ASP.NET 2,0 próbuje znaleźć plik o nazwie `eurl.axd` i wykonać go. Ponieważ taki plik nie istnieje, żądanie kończy się niepowodzeniem z wyjątkiem **HttpException** .

Ten problem można obejść, korzystając z jednej z następujących opcji.

### <a name="option-1"></a>Opcja 1

Jeśli ASP.NET 4 nie jest wymagany do uruchomienia witryny sieci Web, należy ponownie zamapować lokację, tak aby korzystała z ASP.NET 2,0.

### <a name="option-2"></a>Opcja 2

Jeśli ASP.NET 4 jest wymagany do uruchomienia witryny sieci Web, Przenieś wszystkie podrzędne katalogi wirtualne ASP.NET 2,0 do innej witryny sieci Web, która jest zamapowana na ASP.NET 2,0.

### <a name="option-3"></a>Opcja 3

Jeśli nie jest to praktyczne, aby ponownie zmapować witrynę sieci Web do ASP.NET 2,0 lub zmienić lokalizację katalogu wirtualnego, jawnie wyłącz przetwarzanie adresów URL bez rozszerzenia w ASP.NET 4. Postępuj zgodnie z następującą procedurą:

1. W rejestrze systemu Windows otwórz następujący węzeł:

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ASP.NET\4.0.30319.0`

1. Utwórz nową wartość **DWORD** o nazwie **EnableExtensionlessUrls**.
2. Ustaw wartość **EnableExtensionlessUrls** na 0. Spowoduje to wyłączenie zachowania bezserwerowego adresu URL.
3. Zapisz wartość rejestru i Zamknij Edytor rejestru.
4. Uruchom narzędzie wiersza polecenia **IISReset** , które powoduje, że program IIS odczytuje nową wartość rejestru.

> [!NOTE]
> Ustawienie **EnableExtensionlessUrls** na 1 włącza zachowanie w adresie URL, którego nie można rozszerzać. Jest to ustawienie domyślne, jeśli nie określono wartości.

<a id="0.1__Toc252995494"></a><a id="0.1__Toc255587643"></a><a id="0.1__Toc256770154"></a><a id="0.1__Toc245724862"></a>

## <a name="event-handlers-might-not-be-not-raised-in-a-default-document-in-iis-7-or-iis-75-integrated-mode"></a>Programy obsługi zdarzeń mogą nie zostać zgłoszone w dokumencie domyślnym w trybie zintegrowanym usług IIS 7 lub IIS 7,5

ASP.NET 4 obejmuje modyfikacje, które zmieniają sposób, w jaki atrybut **Action** elementu **formularza** HTML jest renderowany, gdy adres URL bez rozszerzenia jest rozpoznawany jako dokument domyślny. Przykładem niemożliwego do rozpoznania adresu URL do dokumentu domyślnego jest [http://contoso.com/](http://contoso.com/), co spowoduje żądanie [http://contoso.com/Default.aspx](http://contoso.com/Default.aspx).

ASP.NET 4 teraz renderuje wartość atrybutu **Action** elementu **formularza** HTML jako pusty ciąg, gdy żądanie zostanie wysłane do adresu URL, który ma przypisany do niego dokument domyślny. Na przykład we wcześniejszych wersjach ASP.NET żądanie do [http://contoso.com](http://contoso.com) mogłoby spowodować żądanie `Default.aspx`. W tym dokumencie tag **formularza** otwierającego będzie renderowany jak w poniższym przykładzie:

`<form action="Default.aspx" />`

W ASP.NET 4 żądanie do [http://contoso.com](http://contoso.com) również skutkuje żądaniem `Default.aspx`. Jednak ASP.NET teraz renderuje tag HTML otwierającego **formularza** tak jak w poniższym przykładzie:

`<form action="" />`

Różnica w sposobie renderowania atrybutu **akcji** może spowodować drobne zmiany w sposobie przetwarzania post formularzy przez usługi IIS i ASP.NET. Gdy atrybut **Action** jest pustym ciągiem, obiekt **DefaultDocumentModule** IIS utworzy żądanie podrzędne do `Default.aspx`. W większości warunków to żądanie podrzędne jest niewidoczne dla kodu aplikacji, a strona `Default.aspx` działa normalnie.

Jednak potencjalna interakcja między zarządzanym kodem i usługami IIS 7 lub IIS 7,5 może spowodować, że zarządzane strony. aspx przestaną działać prawidłowo podczas żądania podrzędnego. W przypadku wystąpienia następujących warunków żądanie podrzędne do dokumentu `Default.aspx` spowoduje błąd lub w nieoczekiwanym zachowaniu:

1. Strona. aspx jest wysyłana do przeglądarki z atrybutem **Action** elementu **formularza** ustawionym na "".
2. Formularz zostanie opublikowany z powrotem do ASP.NET.
3. Zarządzany moduł HTTP odczytuje część treści jednostki. Na przykład moduł odczytuje element **Request. form** lub **Request. params**. Powoduje to, że treść jednostki żądania POST ma zostać odczytana do pamięci zarządzanej. W związku z tym treść jednostki nie jest już dostępna dla żadnych modułów kodu natywnego, które działają w trybie zintegrowanym usług IIS 7 lub IIS 7,5.
4. Obiekt **DEFAULTDOCUMENTMODULE** IIS zostanie uruchomiony i tworzy żądanie podrzędne do dokumentu `Default.aspx`. Jednak ponieważ treść jednostki została już odczytana przez fragment kodu zarządzanego, nie istnieje żadna treść jednostki do wysłania do żądania podrzędnego.
5. Gdy potok HTTP jest uruchamiany dla żądania podrzędnego, program obsługi dla `.aspx` plików działa podczas fazy wykonywania procedury obsługi.
6. Ponieważ nie ma treści jednostki, nie ma żadnych zmiennych formularzy i nie ma stanu widoku, dlatego nie są dostępne żadne informacje dla programu obsługi stron. aspx, aby określić, jakie zdarzenie (jeśli istnieje) ma zostać zgłoszone. W związku z tym żaden z programów obsługi zdarzeń ogłaszania zwrotnego nie zostanie uruchomiony na stronie, której dotyczy.

To zachowanie można obejść w następujący sposób:

- Zidentyfikuj moduł HTTP, który uzyskuje dostęp do treści jednostki żądania podczas żądania dokumentu domyślnego i określ, czy można go skonfigurować do uruchamiania tylko dla zarządzanych żądań. W trybie zintegrowanym zarówno dla usług IIS 7, jak i IIS 7,5, moduły HTTP można oznaczyć do uruchamiania tylko dla zarządzanych żądań, dodając następujący atrybut do wpisu **System. WebServer/modules** modułu:

- `precondition="managedHandler"`

- To ustawienie powoduje wyłączenie modułu dla żądań, które usługi IIS 7 i IIS 7,5 określają, że nie są to żądania zarządzane. W przypadku żądań dokumentów domyślnych pierwsze żądanie dotyczy bezwzględnego adresu URL. W związku z tym usługi IIS nie uruchamiają żadnych modułów zarządzanych, które są oznaczone jako warunek wstępny programu obsługi zarządzanej podczas wstępnego przetwarzania żądań. W związku z tym moduły zarządzane nie będą przypadkowo odczytywać treści jednostki i w związku z tym treść jednostki jest nadal dostępna i jest przesyłana do żądania podrzędnego i do dokumentu domyślnego.

- Jeśli problematyczne moduły HTTP mają być uruchamiane dla wszystkich żądań (dla plików statycznych, w przypadku adresów URL bez rozszerzenia, które są rozpoznawane jako obiekt **DefaultDocumentModule** , dla żądań zarządzanych itp.), należy zmodyfikować strony, których dotyczy problem, przez jawne ustawienie właściwości **Action** elementu **System. Web. UI. HtmlControls. parametr HtmlForm** na niepusty ciąg. Na przykład, jeśli dokument domyślny jest `Default.aspx`, zmodyfikuj kod strony, aby jawnie ustawić właściwość **akcji** kontrolki **parametr HtmlForm** na "default. aspx".

<a id="0.1__Toc255587644"></a><a id="0.1__Toc256770155"></a>

## <a name="changes-to-the-aspnet-code-access-security-cas-implementation"></a>Zmiany implementacji zabezpieczenia dostępu kodu ASP.NET (CAS)

ASP.NET 2,0 i rozszerzenie funkcji ASP.NET, które zostały dodane w 3,5, użyj modelu zabezpieczeń .NET Framework 1,1 i 2,0 (CAS). Jednakże implementacja urzędów certyfikacji w ASP.NET 4 została znacząco przestawiona. Z tego powodu aplikacje ASP.NET częściowo zaufane, które opierają się na zaufanym kodzie uruchomionym w globalnej pamięci podręcznej zestawów (GAC), mogą zakończyć się niepowodzeniem z różnymi wyjątkami zabezpieczeń. Aplikacje częściowej relacji zaufania, które opierają się na rozległych modyfikacjach zasad CAS komputera, mogą również kończyć się niepowodzeniem z wyjątkami zabezpieczeń.

Można przywrócić częściowe zaufanie ASP.NET 4 aplikacji do zachowania ASP.NET 1,1 i 2,0 przy użyciu nowego atrybutu **legacyCasModel** w elemencie konfiguracji **zaufania** , jak pokazano w następującym przykładzie:

`<trust level= "Medium" legacyCasModel="true" />`

W przypadku powrotu do starszego modelu CAS są włączone następujące stare działania dotyczące urzędów certyfikacji:

- Zasady urzędów certyfikacji maszyn są honorowane.
- Dozwolone jest stosowanie wielu różnych zestawów uprawnień w jednej domenie aplikacji.
- Jawne potwierdzenia uprawnień nie są wymagane dla zestawów w pamięci podręcznej, które są wywoływane, gdy na stosie jest tylko ASP.NET lub inny kod .NET Framework.

Nie można przywrócić jednego scenariusza w .NET Framework 4: aplikacje częściowej relacji zaufania spoza sieci Web nie mogą już wywoływać niektórych interfejsów API w bibliotekach system. Web. dll i system. Web. Extensions. dll. W poprzednich wersjach .NET Framework było możliwe, aby aplikacje niebędące częściowym zaufaniem z sieci Web były jawnie przyznawane uprawnienia **AspNetHostingPermission** . Aplikacje te mogą następnie używać **System. Web. HttpUtility**, typy w przestrzeniach nazw **System. Web. ClientServices.\*** i typy związane z członkostwem, rolami i profilami. Wywoływanie tych typów z aplikacji częściowo zaufanych spoza sieci Web nie jest już obsługiwane w .NET Framework 4.

> [!NOTE]
> Funkcja **HtmlEncode** i **HtmlDecode** klasy **System. Web. HttpUtility** została przeniesiona do nowej klasy .NET Framework 4 **System .NET. WebUtility** . Jeśli była to jedyna używana funkcja ASP.NET, zmodyfikuj kod aplikacji tak, aby korzystał z nowej klasy **narzędzi WebUtility** .

Poniżej znajduje się podsumowanie zmian w domyślnej implementacji CAS w ASP.NET 4:

- Domeny aplikacji ASP.NET są teraz jednorodnymi domenami aplikacji. W domenie aplikacji są dostępne tylko zestawy dotacji częściowo-zaufania i pełnego zaufania.
- Zestawy dotacji ASP.NET częściowej zaufania są niezależne od dowolnych zasad CAS na poziomie komputera lub na poziomie użytkownika.
- ASP.NET zestawy, które zostały dostarczone w 3,5 i 3,5 SP1, zostały przekonwertowane do korzystania z modelu przezroczystości .NET Framework 4.
- Użycie atrybutu ASP.NET **AspNetHostingPermission** zostało znacznie ograniczone. Większość wystąpień tego atrybutu została usunięta z publicznych interfejsów API ASP.NET.
- Dynamicznie skompilowane zestawy tworzone przez dostawców kompilacji ASP.NET zostały zaktualizowane, aby jawnie oznaczyć zestawy jako przezroczyste.
- Wszystkie zestawy ASP.NET są teraz oznaczone w taki sposób, że atrybut APTCA jest uznawany tylko za środowiska hostingu w sieci Web. Częściowo zaufane środowiska hostingu niezwiązane z siecią Web, takie jak ClickOnce, nie będą mogły wywołać zestawów ASP.NET.

Aby uzyskać więcej informacji na temat nowego modelu zabezpieczeń dostępu kodu ASP.NET 4, zobacz [Używanie zabezpieczeń dostępu kodu w aplikacjach ASP.NET](https://msdn.microsoft.com/library/dd984947%28VS.100%29.aspx) w witrynie MSDN w sieci Web.

<a id="0.1__Toc256770156"></a><a id="0.1__Toc245724863"></a><a id="0.1__Toc252995496"></a><a id="0.1__Toc255587645"></a><a id="0.1__Toc245724864"></a>

## <a name="membershipuser-and-other-types-in-the-systemwebsecurity-namespace-have-been-moved"></a>MembershipUser i inne typy w przestrzeni nazw System. Web. Security zostały przeniesione

Niektóre typy, które są używane w członkostwie ASP.NET, zostały przeniesione z `System.Web.dll` do nowego zestawu System. Web. ApplicationServices. dll. Typy zostały przeniesione w celu rozwiązania zależności między typami w kliencie a rozszerzonymi jednostkami SKU .NET Framework.

Projekty witryn sieci Web nie mają problemów wynikających z przeniesienia tych typów, ponieważ system. Web. ApplicationServices. dll został dodany do listy przywoływanych zestawów, które są używane domyślnie przez system kompilacji ASP.NET. W przypadku uaktualniania projektu witryny sieci Web, który został utworzony przy użyciu wcześniejszej wersji programu ASP.NET do ASP.NET 4, otwierając go w programie Visual Studio 2010, projekt zostanie skompilowany bez błędów.

Podobnie w przypadku uaktualniania projektu aplikacji sieci Web, który został utworzony we wcześniejszej wersji programu ASP.NET do ASP.NET 4 przez otwarcie go w programie Visual Studio 2010, proces uaktualniania dodaje odwołanie do elementu System. Web. ApplicationServices. dll do projektu. W związku z tym uaktualnione projekty aplikacji sieci Web również zostaną skompilowane bez błędów.

Skompilowane (binarne) pliki, które zostały utworzone przy użyciu wcześniejszych wersji programu ASP.NET, również zostaną uruchomione bez błędów w ASP.NET 4, mimo że typy członkostwa zostały przeniesione do innego zestawu. Informacje o przekazywaniu typu zostały dodane do wersji ASP.NET 4 `System.Web.dll`, która automatycznie kieruje odwołania w czasie wykonywania dla tych typów do nowej lokalizacji dla typów.

Jednak biblioteki klas korzystające z określonych typów członkostwa i uaktualnione z wcześniejszych wersji ASP.NET nie będą kompilowane, gdy są używane w projekcie ASP.NET 4. Na przykład projekt biblioteki klas może zakończyć się niepowodzeniem i zgłosić błąd, taki jak następujące:

- `The type 'System.Web.Security.MembershipUser' is defined in an assembly that is not referenced. You must add a reference to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`

- `The type name 'MembershipUser' could not be found. This type has been forwarded to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'. Consider adding a reference to that assembly.`

Można obejść ten problem, dodając odwołanie w projekcie biblioteki klas do System. Web. ApplicationServices. dll.

Na poniższej liście przedstawiono typy *System. Web. Security* , które zostały przeniesione z `System.Web.dll` do System. Web. ApplicationServices. dll:

- *System. Web. Security. MembershipCreateStatus*
- *System. Web. Security. Membership. dbuserexception*
- *System. Web. Security. MembershipPasswordException —*
- *System. Web. Security. MembershipPasswordFormat*
- *System. Web. Security. MembershipProvider*
- *System. Web. Security. MembershipProviderCollection*
- *System. Web. Security. MembershipUser*
- *System. Web. Security. MembershipUserCollection*
- *System. Web. Security. MembershipValidatePasswordEventHandler*
- *System. Web. Security. ValidatePasswordEventArgs*
- *System. Web. Security. RoleProvider*
- <a id="0.1_a"></a>*System. Web. Configuration. MembershipPasswordCompatibilityMode*

<a id="0.1__Toc256770157"></a>

## <a name="output-caching-changes-to-vary--http-header"></a>Buforowanie zmian w danych wyjściowych w różnych \* nagłówku HTTP

W programie ASP.NET 1,0 usterka spowodowała, że w pamięci podręcznej określone `Location="ServerAndClient"` jako dane wyjściowe — ustawienie w pamięci podręcznej, które wyemituje `Vary:*` nagłówek HTTP w odpowiedzi. Miało to wpływ na to, że przeglądarki klienta nigdy nie buforują strony lokalnie.

W ASP.NET 1,1 Dodano metodę **System. Web. HttpCachePolicy. SetOmitVaryStar** , którą można wywołać, aby pominąć nagłówek `Vary:*`. Ta metoda została wybrana, ponieważ zmiana wyemitowanego nagłówka HTTP została uznana za potencjalnie nieprzerwaną zmianę w danym momencie. Jednak deweloperzy zostały pomylone przez zachowanie w programie ASP.NET, a raporty o błędach sugerują, że deweloperzy nie znają istniejących zachowań **SetOmitVaryStar** .

W ASP.NET 4 podjęto decyzję o usunięciu problemu głównego. Nagłówek `Vary:*` HTTP nie jest już emitowany z odpowiedzi, które określają następującą dyrektywę:

`<%@OutputCache Location="ServerAndClient" %>`

W związku z tym **SetOmitVaryStar** nie jest już potrzebne, aby pominąć nagłówek `Vary:*`.

W aplikacjach, które określają `Location="ServerAndClient"` w dyrektywie **@ OutputCache** na stronie, zobaczysz zachowanie implikowane przez nazwę wartości atrybutu **lokalizacji** — to oznacza, że strony będą buforowane w przeglądarce bez konieczności wywoływania metody **SetOmitVaryStar** .

Jeśli strony w aplikacji muszą emitować `Vary:*`, wywołaj metodę **AppendHeader** , jak w poniższym przykładzie:

`HttpResponse.AppendHeader("Vary","*");`

Alternatywnie można zmienić wartość atrybutu **lokalizacji** buforowania danych wyjściowych na "serwer".

<a id="0.1__Toc255587646"></a><a id="0.1__Toc256770158"></a>

## <a name="systemwebsecurity-types-for-passport-are-obsolete"></a>Typy system. Web. Security dotyczące usługi Passport są przestarzałe

Obsługa paszportów wbudowana w ASP.NET 2,0 była przestarzała i nieobsługiwana przez kilka lat ze względu na zmiany w usłudze Passport (teraz LiveID). W związku z tym pięć typów związanych z paszportem w **systemie System. Web. Security** są teraz oznaczone atrybutem **ObsoleteAttribute** .

<a id="0.1__The_MenuItem.PopOutImageUrl_Propert"></a><a id="0.1__Toc256770159"></a>

## <a name="the-menuitempopoutimageurl-property-fails-to-render-an-image-in-aspnet-4"></a>Właściwość MenuItem. PopOutImageUrl nie może renderować obrazu w ASP.NET 4

W ASP.NET 3,5 Właściwość *MenuItem. PopOutImageUrl* pozwala określić adres URL obrazu, który jest wyświetlany w elemencie menu, aby wskazać, że element menu zawiera dynamiczne podmenu. Poniższy przykład pokazuje, jak określić tę właściwość w adjustacji w ASP.NET 3,5.

[!code-aspx[Main](breaking-changes/samples/sample11.aspx)]

W wyniku zmiany projektu w ASP.NET 4, żadne wyjście nie jest renderowane dla *PopOutImageUrl* , jeśli właściwość jest ustawiona dla klasy *MenuItem* . Zamiast tego należy określić adres URL obrazu bezpośrednio w kontrolce *menu* przy użyciu właściwości *StaticPopOutImageUrl* lub właściwości *DynamicPopOutImageUrl* . Podczas pracy z menu statycznym Właściwość *menu. StaticPopOutImageUrl* określa adres URL obrazu, który jest wyświetlany w celu wskazania, że statyczny element menu ma podmenu, jak pokazano w następującym przykładzie:

[!code-aspx[Main](breaking-changes/samples/sample12.aspx)]

Jeśli pracujesz z menu dynamicznym, użyj właściwości *menu. DynamicPopOutImageUrl* , aby określić adres URL obrazu, który wskazuje, że element menu dynamicznego ma podmenu. Poniższy przykład jest podobny do poprzedniego, ale pokazuje, jak ustawić właściwość *DynamicPopOutImageUrl* dla menu dynamicznego.

[!code-aspx[Main](breaking-changes/samples/sample13.aspx)]

Jeśli właściwość *menu. DynamicPopOutImageUrl* nie jest ustawiona, a właściwość *menu. DynamicEnableDefaultPopOutImage* ma wartość *false*, obraz nie jest wyświetlany. Podobnie, jeśli właściwość *StaticPopOutImageUrl* nie jest ustawiona, a właściwość *StaticEnableDefaultPopOutImage* ma wartość *false*, nie jest wyświetlany obraz.

Po ustawieniu ścieżek dla tych właściwości Użyj ukośnika (/) zamiast ukośnika odwrotnego (\). Aby uzyskać więcej informacji, zobacz [menu. StaticPopOutImageUrl i menu. DynamicPopOutImageUrl nie renderuje obrazów, gdy ścieżki zawierają ukośniki odwrotne](#0.1__Menu.StaticPopOutImageUrl_and_Menu. "_Menu. StaticPopOutImageUrl_and_Menu.") w tym dokumencie.

<a id="0.1__Menu.StaticPopOutImageUrl_and_Menu."></a><a id="0.1__Toc256770160"></a>

## <a name="menustaticpopoutimageurl-and-menudynamicpopoutimageurl-fail-to-render-images-when-paths-contain-backslashes"></a>Menu. StaticPopOutImageUrl i menu. DynamicPopOutImageUrl nie renderuje obrazów, gdy ścieżki zawierają ukośniki odwrotne

W ASP.NET 4 obrazy określone przy użyciu *menu. StaticPopOutImageUrl* i *menu. DynamicPopOutImageUrl* nie będą renderowane, jeśli ścieżka zawiera nadwiązania (\). Jest to zmiana z wcześniejszych wersji programu ASP.NET.

Poniższy przykład znaczników kontrolki *menu* przedstawia Właściwość *StaticPopOutImageUrl* ustawioną przy użyciu ścieżki zawierającej ukośnik odwrotny. W ASP.NET 4 obraz określony we właściwości nie będzie renderowany.

[!code-aspx[Main](breaking-changes/samples/sample14.aspx)]

Aby rozwiązać ten problem, Zmień wartości ścieżki, które są określone we właściwościach *StaticPopOutImageUrl* i *DynamicPopOutImageUrl* , aby użyć ukośników (/). W poniższym przykładzie przedstawiono tę zmianę:

[!code-aspx[Main](breaking-changes/samples/sample15.aspx)]

Zwróć uwagę na to, że w przypadku aplikacji, które zostały zmigrowane z wcześniejszych wersji programu ASP.NET do ASP.NET 4, można również uwzględnić, ponieważ właściwość *MenuItem. PopOutImageUrl* została zmieniona. Aby uzyskać więcej informacji, zobacz [Właściwość MenuItem. PopOutImageUrl nie może renderować obrazu w ASP.NET 4](#0.1__The_MenuItem.PopOutImageUrl_Propert "_The_MenuItem. PopOutImageUrl_Propert") w tym dokumencie.

<a id="0.1__Toc224729061"></a><a id="0.1__Toc255587647"></a><a id="0.1__Toc256770161"></a>

## <a name="disclaimer"></a>Zastrzeżenie

Ten dokument jest wstępny i może ulec zmianie w odróżnieniu od ostatecznej wersji handlowej oprogramowania opisanego w niniejszej umowie.

Informacje zawarte w tym dokumencie przedstawiają bieżący widok firmy Microsoft Corporation dotyczącej omawianych problemów w dniu publikacji. Ponieważ firma Microsoft musi reagować na zmieniające się warunki rynkowe, nie powinna być interpretowana jako zobowiązanie w ramach firmy Microsoft, a firma Microsoft nie gwarantuje dokładności informacji przedstawionych po dacie publikacji.

Ten oficjalny dokument jest przeznaczony wyłącznie do celów informacyjnych. FIRMA MICROSOFT NIE UDZIELA ŻADNYCH GWARANCJI, WYRAŹNYCH, DOROZUMIANYCH ANI USTAWOWYCH, W ODNIESIENIU DO INFORMACJI ZAWARTYCH W TYM DOKUMENCIE.

Zgodnie ze wszystkimi obowiązującymi prawami autorskimi użytkownik jest odpowiedzialny za użytkownika. Bez ograniczania prawa w ramach praw autorskich, żadna część tego dokumentu nie może zostać odbudowana, zapisana lub wprowadzona w systemie pobierania ani przesłana w jakiejkolwiek formie ani w jakikolwiek sposób (elektroniczny, mechaniczny, z kopiowaniem, nagrań lub w inny sposób) lub w dowolnym celu. bez wyraźnej pisemnej zgody firmy Microsoft Corporation.

Firma Microsoft może mieć patenty, zgłoszenia patentowe, znaki towarowe, prawa autorskie lub inne prawa własności intelektualnej, w tym dokument. O ile nie zostało to wyraźnie określone w pisemnej umowie licencyjnej firmy Microsoft, dostarczenie tego dokumentu nie daje Licencjobiorcy żadnych licencji na te patenty, znaki towarowe, prawa autorskie lub inną własność intelektualną.

O ile nie zaznaczono inaczej, Przykładowe firmy, organizacje, produkty, nazwy domen, adresy e-mail, logo, osoby, miejsca i zdarzenia wymienione w tym dokumencie są fikcyjne i nie są powiązane z rzeczywistymi firmami, organizacjami, produktami, nazwami domen, adresami e-mail adres, logo, osoba, miejsce lub zdarzenie są zamierzone lub wywnioskowane.

© 2010 Microsoft Corporation. Wszelkie prawa zastrzeżone.

Microsoft i Windows są zastrzeżonymi znakami towarowymi lub znakami towarowymi firmy Microsoft Corporation w Stany Zjednoczone i/lub innych krajach.

Nazwy rzeczywistych firm i produktów wymienionych w niniejszym dokumencie mogą być znakami towarowymi odpowiednich właścicieli.

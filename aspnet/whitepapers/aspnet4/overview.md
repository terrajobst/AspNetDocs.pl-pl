---
uid: whitepapers/aspnet4/overview
title: Omówienie programów ASP.NET 4 i Visual Studio 2010 Web Development | Microsoft Docs
author: rick-anderson
description: Ten dokument zawiera omówienie wielu nowych funkcji ASP.NET, które znajdują się w the.NET Framework 4 i w programie Visual Studio 2010.
ms.author: riande
ms.date: 02/10/2010
ms.assetid: d7729af4-1eda-4ff2-8b61-dbbe4fc11d10
msc.legacyurl: /whitepapers/aspnet4
msc.type: content
ms.openlocfilehash: ecde48f6bd88ee5f569bfeb8b70c26a50bc869c2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78630173"
---
# <a name="aspnet-4-and-visual-studio-2010-web-development-overview"></a>Omówienie programowania dla Internetu na platformie ASP.NET 4 i w programie Visual Studio 2010

> Ten dokument zawiera omówienie wielu nowych funkcji ASP.NET, które znajdują się w the.NET Framework 4 i w programie Visual Studio 2010.
> 
> [Pobierz ten oficjalny dokument](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_and_Visual_Studio_2010_Web_Development_Overview.pdf)

**Zawartość**

**[Podstawowe usługi](#0.2__Toc253429238 "_Toc253429238")**  
[Refaktoryzacja pliku Web. config](#0.2__Toc253429239 "_Toc253429239")  
[Rozszerzalne buforowanie danych wyjściowych](#0.2__Toc253429240 "_Toc253429240")  
[Autouruchamianie aplikacji sieci Web](#0.2__Toc253429241 "_Toc253429241")  
[Trwałe przekierowywanie strony](#0.2__Toc253429242 "_Toc253429242")  
[Zmniejszanie stanu sesji](#0.2__Toc253429243 "_Toc253429243")  
[Rozszerzanie zakresu dozwolonych adresów URL](#0.2__Toc253429244 "_Toc253429244")  
[Rozszerzalne sprawdzanie poprawności żądań](#0.2__Toc253429245 "_Toc253429245")  
[Buforowanie obiektów i rozszerzalność buforowania obiektów](#0.2__Toc253429246 "_Toc253429246")  
[Rozszerzalne kodowanie HTML, adresu URL i nagłówka HTTP](#0.2__Toc253429247 "_Toc253429247")  
[Monitorowanie wydajności dla poszczególnych aplikacji w pojedynczym procesie roboczym](#0.2__Toc253429248 "_Toc253429248")  
[Wiele elementów docelowych](#0.2__Toc253429249 "_Toc253429249")

**[Języki](#0.2__Toc253429250 "_Toc253429250")**  
[jQuery dołączone do formularzy sieci Web i MVC](#0.2__Toc253429251 "_Toc253429251")  
[Obsługa Content Delivery Network](#0.2__Toc253429252 "_Toc253429252")  
[Jawne skrypty ScriptManager](#0.2__Toc253429253 "_Toc253429253")

**[Formularze sieci Web](#0.2__Toc253429256 "_Toc253429256")**  
[Ustawianie tagów Meta przy użyciu właściwości Page. metas i Page. dbdescription](#0.2__Toc253429257 "_Toc253429257")  
[Włączanie stanu widoku dla poszczególnych kontrolek](#0.2__Toc253429258 "_Toc253429258")  
[Zmiany w możliwościach przeglądarki](#0.2__Toc253429259 "_Toc253429259")  
[Routing w ASP.NET 4](#0.2__Toc253429260 "_Toc253429260")  
[Ustawianie identyfikatorów klientów](#0.2__Toc253429261 "_Toc253429261")  
[Utrwalanie wyboru wierszy w kontrolkach danych](#0.2__Toc253429262 "_Toc253429262")  
[Kontrolka wykresu ASP.NET](#0.2__Toc253429263 "_Toc253429263")  
[Filtrowanie danych za pomocą kontrolki QueryExtender](#0.2__Toc253429264 "_Toc253429264")  
[Wyrażenia kodu kodowane HTML](#0.2__Toc253429265 "_Toc253429265")  
[Zmiany szablonu projektu](#0.2__Toc253429266 "_Toc253429266")  
[Udoskonalenia CSS](#0.2__Toc253429267 "_Toc253429267")  
[Ukrywanie elementów div wokół ukrytych pól](#0.2__Toc253429268 "_Toc253429268")  
[Renderowanie zewnętrznej tabeli dla kontrolek z szablonami](#0.2__Toc253429269 "_Toc253429269")  
[Usprawnienia formantów ListView](#0.2__Toc253429270 "_Toc253429270")  
[Udoskonalenia formantów formant CheckBoxList i RadioButtonList](#0.2__Toc253429271 "_Toc253429271")  
[Ulepszenia kontrolek menu](#0.2__Toc253429272 "_Toc253429272")  
[Kreator i kontrolki formancie CreateUserWizard 56](#0.2__Toc253429273 "_Toc253429273")

**[ASP.NET MVC](#0.2__Toc253429274 "_Toc253429274")**  
[Obsługa obszarów](#0.2__Toc253429275 "_Toc253429275")  
[Obsługa walidacji atrybutów adnotacji z danymi](#0.2__Toc253429276 "_Toc253429276")  
[Pomocnicy z szablonami](#0.2__Toc253429277 "_Toc253429277")

**[Dane dynamiczne](#0.2__Toc253429278 "_Toc253429278")**  
[Włączanie danych dynamicznych dla istniejących projektów](#0.2__Toc253429279 "_Toc253429279")  
[Deklaratywna składnia formantów Menedżera danych dynamicznych](#0.2__Toc253429280 "_Toc253429280")  
[Szablony jednostek](#0.2__Toc253429281 "_Toc253429281")  
[Nowe szablony pól dla adresów URL i adresów E-mail](#0.2__Toc253429282 "_Toc253429282")  
[Tworzenie linków z kontrolką formantem DynamicHyperLink](#0.2__Toc253429283 "_Toc253429283")  
[Obsługa dziedziczenia w modelu danych](#0.2__Toc253429284 "_Toc253429284")  
[Obsługa relacji wiele-do-wielu (tylko Entity Framework)](#0.2__Toc253429285 "_Toc253429285")  
[Nowe atrybuty do kontroli wyświetlania i obsługi wyliczeń](#0.2__Toc253429286 "_Toc253429286")  
[Ulepszona obsługa filtrów](#0.2__Toc253429287 "_Toc253429287")

**[Ulepszenia programu Visual Studio 2010 Web Development](#0.2__Toc253429288 "_Toc253429288")**  
[Lepsza zgodność stylów CSS](#0.2__Toc253429289 "_Toc253429289")  
[Fragmenty kodu HTML i JavaScript](#0.2__Toc253429290 "_Toc253429290")  
[Ulepszenia funkcji IntelliSense języka JavaScript](#0.2__Toc253429291 "_Toc253429291")

**[Wdrażanie aplikacji sieci Web za pomocą programu Visual Studio 2010](#0.2__Toc253429292 "_Toc253429292")**  
[Pakiet internetowy](#0.2__Toc253429293 "_Toc253429293")  
[Transformacja pliku Web. config](#0.2__Toc253429294 "_Toc253429294")  
[Wdrażanie bazy danych](#0.2__Toc253429295 "_Toc253429295")  
[Publikowanie aplikacji sieci Web jednym kliknięciem](#0.2__Toc253429296 "_Toc253429296")  
[Zasoby](#0.2__Toc253429297 "_Toc253429297")

**[Zastrzeżenie](#0.2__Toc253429298 "_Toc253429298")**

<a id="0.2__Toc224729018"></a><a id="0.2__Toc253429238"></a><a id="0.2__Toc243304612"></a>

## <a name="core-services"></a>Podstawowe usługi

ASP.NET 4 zawiera szereg funkcji ulepszających podstawowe usługi ASP.NET, takie jak buforowanie danych wyjściowych i magazyn stanów sesji.

<a id="0.2__Toc243304613"></a><a id="0.2__Toc253429239"></a><a id="0.2__Toc224729019"></a>

### <a name="webconfig-file-refactoring"></a>Refaktoryzacja pliku Web. config

Plik `Web.config`, który zawiera konfigurację aplikacji sieci Web, znacznie zwiększył się w ciągu ostatnich kilku wersji .NET Framework, ponieważ dodano nowe funkcje, takie jak AJAX, Routing i integracja z usługami IIS 7. Dzięki temu trudniej jest skonfigurować lub uruchomić nowe aplikacje sieci Web bez narzędzia takiego jak Visual Studio. W programie .NET Framework 4 elementy konfiguracji głównej zostały przeniesione do pliku `machine.config`, a aplikacje teraz dziedziczą te ustawienia. Dzięki temu plik `Web.config` w aplikacjach ASP.NET 4 musi być pusty lub zawierać tylko następujące wiersze, które określają dla programu Visual Studio wersję platformy, do której ma zastosowanie aplikacja:

[!code-xml[Main](overview/samples/sample1.xml)]

<a id="0.2__Toc253429240"></a><a id="0.2__Toc243304614"></a>

### <a name="extensible-output-caching"></a>Rozszerzalne buforowanie danych wyjściowych

Od czasu wydania ASP.NET 1,0, buforowanie danych wyjściowych umożliwiło deweloperom przechowywanie generowanych danych wyjściowych stron, kontrolek i odpowiedzi HTTP w pamięci. W kolejnych żądaniach sieci Web ASP.NET może szybciej obsłużyć zawartość, pobierając wygenerowane dane wyjściowe z pamięci zamiast ponownie wygenerować dane wyjściowe od początku. Jednak takie podejście ma ograniczenie — wygenerowana zawartość zawsze musi być przechowywana w pamięci i na serwerach, na których występuje duży ruch, pamięć wykorzystywana przez buforowanie danych wyjściowych może konkurować z zapotrzebowaniem pamięci z innych części aplikacji sieci Web.

ASP.NET 4 dodaje punkty rozszerzalności do buforowania danych wyjściowych, które umożliwiają skonfigurowanie co najmniej jednego niestandardowego dostawcy pamięci podręcznej. Dostawcy pamięci podręcznej magazynu mogą używać dowolnego mechanizmu magazynowania, aby zachować zawartość HTML. Dzięki temu można utworzyć niestandardowych dostawców pamięci podręcznej dla różnorodnych mechanizmów trwałości, takich jak dyski lokalne lub zdalne, magazyn w chmurze i rozproszone aparaty pamięci podręcznej.

Utwórz niestandardowego dostawcę wyjściowej pamięci podręcznej jako klasę, która pochodzi od nowego typu *System. Web. cache. OutputCacheProvider* . Następnie można skonfigurować dostawcę w pliku `Web.config` przy użyciu nowej podsekcji *providers* elementu *OutputCache* , jak pokazano w następującym przykładzie:

[!code-xml[Main](overview/samples/sample2.xml)]

Domyślnie w ASP.NET 4 wszystkie odpowiedzi HTTP, renderowane strony i kontrolki używają wyjściowej pamięci podręcznej w pamięci, jak pokazano w poprzednim przykładzie, gdzie atrybut *wartości właściwości defaultProvider* jest ustawiony na AspNetInternalProvider. Można zmienić domyślnego dostawcę pamięci podręcznej danych wyjściowych używany dla aplikacji sieci Web, określając inną nazwę dostawcy dla *wartości właściwości defaultProvider*.

Ponadto można wybrać różne dostawcy wyjściowej pamięci podręcznej na kontrolkę i na żądanie. Najprostszym sposobem wybrania innego dostawcy wyjściowej pamięci podręcznej dla różnych kontrolek użytkownika sieci Web jest zdecydowanie użycie nowego atrybutu *ProviderName* w dyrektywie kontrolnej, jak pokazano w następującym przykładzie:

[!code-aspx[Main](overview/samples/sample3.aspx)]

Określenie innego dostawcy wyjściowej pamięci podręcznej dla żądania HTTP wymaga nieco więcej pracy. Zamiast deklaratywnego określania dostawcy, należy zastąpić nową metodę *GetOuputCacheProviderName* w pliku `Global.asax`, aby programowo określić dostawcę, który ma być używany dla określonego żądania. W przykładzie poniżej pokazano, jak to zrobić.

[!code-csharp[Main](overview/samples/sample4.cs)]

Dzięki dodaniu rozszerzalności dostawcy pamięci podręcznej do ASP.NET 4 można teraz bardziej agresywne i bardziej inteligentne strategie buforowania danych wyjściowych dla witryn sieci Web. Na przykład teraz można buforować strony "10 najważniejszych" w lokacji w pamięci, podczas gdy buforowanie stron, które uzyskują mniejszy ruch na dysku. Alternatywnie można buforować co różne kombinacje dla renderowanej strony, ale używać rozproszonej pamięci podręcznej w celu odciążenia zużycia pamięci z serwerów frontonu sieci Web.

<a id="0.2__Toc224729020"></a><a id="0.2__Toc253429241"></a><a id="0.2__Toc243304615"></a>

### <a name="auto-start-web-applications"></a>Autouruchamianie aplikacji sieci Web

Niektóre aplikacje sieci Web muszą ładować duże ilości danych lub wykonywać kosztowne przetwarzanie inicjacji przed rozpoczęciem pierwszego żądania. We wcześniejszych wersjach programu ASP.NET, w tych sytuacjach, należało opracować niestandardowe podejścia do "wznawiania" aplikacji ASP.NET, a następnie uruchomić kod inicjujący podczas *\_aplikacji Metoda ładowania* w pliku `Global.asax`.

Nowa funkcja skalowalności o nazwie *Autostart* , która bezpośrednio dotyczy tego scenariusza, jest dostępna, gdy ASP.NET 4 działa w usługach IIS 7,5 w systemie Windows Server 2008 R2. Funkcja Autostart zawiera kontrolowane podejście do uruchamiania puli aplikacji, inicjowania aplikacji ASP.NET, a następnie akceptowania żądań HTTP.

> [!NOTE] 
> 
> Moduł rozgrzewania aplikacji usług IIS dla usług IIS 7,5
> 
> Zespół usług IIS wydał pierwszą wersję testową wersji beta modułu rozgrzewania aplikacji dla usług IIS 7,5. Dzięki temu aplikacje są rozgrzewane jeszcze lepiej niż wcześniej. Zamiast pisać kod niestandardowy, należy określić adresy URL zasobów do wykonania przed zaakceptowaniem przez aplikację sieci Web żądań z sieci. Ta rozgrzewanie występuje podczas uruchamiania usługi IIS (w przypadku skonfigurowania puli aplikacji usług IIS jako *AlwaysRunning*) i czasu odtwarzania procesu roboczego usług IIS. Podczas odtwarzania stary proces roboczy usług IIS nadal wykonuje żądania do momentu, gdy nowo zduplikowany proces roboczy jest w pełni rozgrzewany, dzięki czemu aplikacje nie będą powodować przerw w działaniu ani innych problemów spowodowanych niektórymi pamięciami podręcznymi. Należy zauważyć, że ten moduł działa w dowolnej wersji programu ASP.NET, począwszy od wersji 2,0.
> 
> Aby uzyskać więcej informacji, zobacz artykuł [rozgrzewanie aplikacji](https://www.iis.net/extensions/applicationwarmup%20on%20the%20IIS.net) w witrynie sieci Web IIS.NET. Aby zapoznać się z przewodnikiem, który ilustruje sposób korzystania z funkcji rozgrzewania, zobacz [wprowadzenie z modułem rozgrzewania aplikacji usług IIS 7,5](https://www.iis.net/learn/manage) w witrynie sieci Web IIS.NET.

Aby można było korzystać z funkcji automatycznego uruchamiania, administrator usług IIS Ustawia pulę aplikacji w usługach IIS 7,5 do automatycznego uruchamiania przy użyciu następującej konfiguracji w pliku `applicationHost.config`:

[!code-xml[Main](overview/samples/sample5.xml)]

Ponieważ jedna pula aplikacji może zawierać wiele aplikacji, należy określić pojedyncze aplikacje do automatycznego uruchamiania przy użyciu następującej konfiguracji w pliku `applicationHost.config`:

[!code-xml[Main](overview/samples/sample6.xml)]

Gdy serwer usług IIS 7,5 jest zimny lub w przypadku odtwarzania pojedynczej puli aplikacji, usługi IIS 7,5 używają informacji w pliku `applicationHost.config`, aby określić, które aplikacje sieci Web mają być uruchamiane automatycznie. Dla każdej aplikacji, która jest oznaczona do autostartu, usługi IIS 7.5 wysyłają żądanie do ASP.NET 4, aby uruchomić aplikację w stanie, w którym aplikacja tymczasowo nie akceptuje żądań HTTP. Gdy jest w tym stanie, ASP.NET tworzy wystąpienie typu zdefiniowanego przez atrybut *serviceAutoStartProvider* (jak pokazano w poprzednim przykładzie) i wywołuje do jego publicznego punktu wejścia.

Można utworzyć zarządzany typ autostartu z niezbędnym punktem wejścia, implementując Interfejs *IProcessHostPreloadClient* , jak pokazano w następującym przykładzie:

[!code-csharp[Main](overview/samples/sample7.cs)]

Gdy kod inicjujący zostanie uruchomiony w metodzie *Preload* , a metoda zwróci metodę, aplikacja ASP.NET jest gotowa do przetwarzania żądań.

Dodanie autostartu do usług IIS .5 i ASP.NET 4 ma teraz dobrze zdefiniowane podejście do wykonywania kosztownych inicjalizacji aplikacji przed przetworzeniem pierwszego żądania HTTP. Na przykład można użyć nowej funkcji Autostart, aby zainicjować aplikację, a następnie sygnalizować moduł równoważenia obciążenia, że aplikacja została zainicjowana i jest gotowa do akceptowania ruchu HTTP.

<a id="0.2__Toc224729021"></a><a id="0.2__Toc253429242"></a><a id="0.2__Toc243304616"></a>

### <a name="permanently-redirecting-a-page"></a>Trwałe przekierowywanie strony

Typowym sposobem działania aplikacji sieci Web jest przenoszenie stron i innych zawartości w czasie, co może prowadzić do akumulacji starych linków w wyszukiwarkach. W programie ASP.NET deweloperzy mają tradycyjnie obsłużone żądania do starych adresów URL przy użyciu metody *Response. Redirect* , aby przesłać żądanie do nowego adresu URL. Jednak Metoda *redirect* wystawia odpowiedź HTTP 302 (tymczasowy przekierowania), która powoduje dodatkową podróż http, gdy użytkownicy próbują uzyskać dostęp do starych adresów URL.

ASP.NET 4 dodaje nową metodę pomocnika *RedirectPermanent* , która ułatwia wystawienie trwałych odpowiedzi HTTP 301, jak w poniższym przykładzie:

[!code-csharp[Main](overview/samples/sample8.cs)]

Aparaty wyszukiwania i inni agenci użytkowników, którzy rozpoznają trwałe przekierowania, będą przechowywali nowy adres URL, który jest skojarzony z zawartością, co eliminuje niezbędną rundę rundy przewidzianą przez przeglądarkę dla tymczasowych przekierowań.

<a id="0.2__Toc224729022"></a><a id="0.2__Toc253429243"></a><a id="0.2__Toc243304617"></a>

### <a name="shrinking-session-state"></a>Zmniejszanie stanu sesji

ASP.NET udostępnia dwie opcje domyślne do przechowywania stanu sesji w kolektywie serwerów sieci Web: Dostawca stanu sesji, który wywołuje serwer stanu sesji poza procesem, oraz dostawcę stanu sesji, który przechowuje dane w Microsoft SQL Server bazie danych. Ponieważ obie opcje obejmują przechowywanie informacji o stanie poza procesem roboczym aplikacji sieci Web, stan sesji musi być serializowany przed wysłaniem do magazynu zdalnego. W zależności od tego, jak dużo informacji jest zapisywany przez dewelopera w stanie sesji, rozmiar serializowanych danych może być bardzo duży.

ASP.NET 4 wprowadza nową opcję kompresji dla obu rodzajów dostawców stanu sesji poza procesem. Gdy opcja konfiguracji *compressionEnabled* pokazana w poniższym przykładzie ma *wartość true*, ASP.NET kompresuje (i dekompresuje) serializowany stan sesji za pomocą klasy .NET Framework *System. IO. Compression. GZipStream* .

[!code-xml[Main](overview/samples/sample9.xml)]

Wraz z prostym dodaniem nowego atrybutu do pliku `Web.config` aplikacje mające zapasowe cykle procesora CPU na serwerach sieci Web mogą mieć znaczny spadek rozmiaru serializowanych danych stanu sesji.

<a id="0.2__Toc253429244"></a><a id="0.2__Toc243304618"></a>

### <a name="expanding-the-range-of-allowable-urls"></a>Rozszerzanie zakresu dozwolonych adresów URL

ASP.NET 4 wprowadza nowe opcje rozszerzania rozmiaru adresów URL aplikacji. Poprzednie wersje ASP.NET ograniczone ścieżki URL o długości do 260 znaków na podstawie limitu ścieżki plików NTFS. W ASP.NET 4 można zwiększyć (lub zmniejszyć) ten limit dla aplikacji przy użyciu dwóch nowych atrybutów konfiguracji *httpRuntime* . Poniższy przykład pokazuje te nowe atrybuty.

[!code-xml[Main](overview/samples/sample10.xml)]

Aby umożliwić dłuższe lub krótsze ścieżki (część adresu URL, która nie zawiera protokołu, nazwy serwera i ciągu zapytania), zmodyfikuj atrybut *[maxUrlLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxurllength.aspx)* . Aby zezwolić na dłuższe lub krótsze ciągi zapytań, zmodyfikuj wartość atrybutu *[MaxQueryStringLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxquerystringlength.aspx)* .

ASP.NET 4 pozwala także skonfigurować znaki, które są używane przez sprawdzanie znaków adresu URL. Gdy ASP.NET odnajdzie nieprawidłowy znak w części adresu URL, odrzuca żądanie i wystawia błąd HTTP 400. We wcześniejszych wersjach programu ASP.NET Sprawdzanie znaków adresu URL było ograniczone do ustalonego zestawu znaków. W ASP.NET 4 można dostosować zestaw prawidłowych znaków przy użyciu nowego atrybutu *RequestPathInvalidCharacters* elementu konfiguracji *httpRuntime* , jak pokazano w następującym przykładzie:

[!code-xml[Main](overview/samples/sample11.xml)]

Domyślnie atrybut *RequestPathInvalidCharacters* definiuje osiem znaków jako nieprawidłowe. (W ciągu, który jest domyślnie przypisany do *RequestPathInvalidCharacters* , znaki mniejsze niż (&lt;), większe niż (&gt;) i handlowe "(&amp;) są kodowane, ponieważ plik `Web.config` jest plikiem XML. W razie konieczności można dostosować zestaw nieprawidłowych znaków.

> [!NOTE]
> Uwaga ASP.NET 4 zawsze odrzuca ścieżki URL zawierające znaki w zakresie ASCII od 0x00 do 0x1F, ponieważ są to nieprawidłowe znaki URL, zgodnie z definicją w dokumencie RFC 2396 grupy IETF ([http://www.ietf.org/rfc/rfc2396.txt](http://www.ietf.org/rfc/rfc2396.txt)). W wersjach systemu Windows Server z uruchomionymi usługami IIS w wersji 6 lub nowszej sterownik urządzenia http. sys automatycznie odrzuca adresy URL z tymi znakami.

<a id="0.2__Toc253429245"></a><a id="0.2__Toc243304619"></a>

### <a name="extensible-request-validation"></a>Rozszerzalne sprawdzanie poprawności żądań

Walidacja żądania ASP.NET przeszukuje przychodzące dane żądań HTTP dla ciągów, które są często używane w atakach obejmujących wiele lokacji (XSS). Jeśli zostaną znalezione potencjalne ciągi XSS, walidacja żądań flaguje podejrzany ciąg i zwraca błąd. Walidacja wbudowanego żądania zwraca błąd tylko wtedy, gdy znajduje najbardziej typowe ciągi używane w atakach XSS. Poprzednie próby nawiązania bardziej agresywnej weryfikacji XSS spowodowały zbyt wiele fałszywych dodatnich. Jednak klienci mogą chcieć zażądać zweryfikowania żądań, które są bardziej agresywne, lub odwrotnie mogą chcieć celowo złagodzić testy XSS dla określonych stron lub dla określonych typów żądań.

W ASP.NET 4 funkcja walidacji żądania została rozszerzona, aby można było użyć niestandardowej logiki walidacji żądania. Aby można było rozciągnąć sprawdzanie poprawności żądania, należy utworzyć klasę pochodzącą od nowego typu *System. Web. util. RequestValidator* , a następnie skonfigurować aplikację (w sekcji *httpRuntime* pliku `Web.config`), aby użyć typu niestandardowego. Poniższy przykład pokazuje, jak skonfigurować niestandardową klasę walidacji żądania:

[!code-xml[Main](overview/samples/sample12.xml)]

Nowy atrybut *RequestValidationType* wymaga standardowego ciągu identyfikatora typu .NET Framework, który określa klasę, która dostarcza niestandardowe żądanie weryfikacji. Dla każdego żądania ASP.NET wywołuje typ niestandardowy, aby przetwarzać każdy element przychodzących danych żądań HTTP. Przychodzący adres URL, wszystkie nagłówki HTTP (pliki cookie i nagłówki niestandardowe) oraz treść jednostki są dostępne do inspekcji przez niestandardową klasę walidacji żądania, jak pokazano w następującym przykładzie:

[!code-csharp[Main](overview/samples/sample13.cs)]

W przypadku, gdy nie chcesz sprawdzać danych przychodzącego protokołu HTTP, Klasa żądania-walidacji może powracać w celu umożliwienia uruchomienia ASP.NET domyślnego żądania weryfikacji przez wywołanie *Base. IsValidRequestString.*

<a id="0.2__Toc253429246"></a><a id="0.2__Toc243304620"></a>

### <a name="object-caching-and-object-caching-extensibility"></a>Buforowanie obiektów i rozszerzalność buforowania obiektów

Od momentu jego pierwszego wydania ASP.NET dołączyła do pamięci podręcznej obiektów w pamięci (*System. Web. cache. pamięci podręcznej*). Implementacja pamięci podręcznej była tak popularna, że została użyta w aplikacjach innych niż aplikacje sieci Web. Jest to jednak niewygodna dla aplikacji Windows Forms lub WPF, aby dołączać odwołanie do `System.Web.dll` tylko do korzystania z pamięci podręcznej obiektów ASP.NET.

Aby udostępnić buforowanie dla wszystkich aplikacji, .NET Framework 4 wprowadza nowy zestaw, nową przestrzeń nazw, typy podstawowe i konkretną implementację pamięci podręcznej. Nowy zestaw `System.Runtime.Caching.dll` zawiera nowy interfejs API buforowania w przestrzeni nazw *System. Runtime. buforowania* . Przestrzeń nazw zawiera dwa podstawowe zestawy klas:

- Typy abstrakcyjne, które stanowią podstawę do tworzenia dowolnego typu implementacji niestandardowej pamięci podręcznej.
- Konkretna implementacja pamięci podręcznej obiektów w pamięci (Klasa *System. Runtime. cache. elemencie MemoryCache* ).

Nowa Klasa *elemencie MemoryCache* jest ściśle modelowana w pamięci podręcznej ASP.NET i udostępnia większość logiki aparatu wewnętrznej pamięci podręcznej z ASP.NET. Mimo że publiczne interfejsy API buforowania w *System. Runtime.* cache zostały zaktualizowane w celu obsługi opracowywania niestandardowych pamięci podręcznych, jeśli użyto obiektu *pamięci podręcznej* ASP.NET, znajdziesz znane koncepcje dotyczące nowych interfejsów API.

Szczegółowe omówienie nowej klasy *elemencie MemoryCache* i obsługi podstawowych interfejsów API wymagały całego dokumentu. Jednak w poniższym przykładzie przedstawiono sposób działania nowego interfejsu API pamięci podręcznej. Przykład został zapisany dla aplikacji Windows Forms bez żadnej zależności od `System.Web.dll`.

[!code-csharp[Main](overview/samples/sample14.cs)]

<a id="0.2__Toc253429247"></a><a id="0.2__Toc243304621"></a>

### <a name="extensible-html-url-and-http-header-encoding"></a>Rozszerzalne kodowanie HTML, adresu URL i nagłówka HTTP

W ASP.NET 4 można utworzyć niestandardowe procedury kodowania dla następujących typowych zadań kodowania tekstu:

- Kodowanie HTML.
- Kodowanie adresu URL.
- Kodowanie atrybutu HTML.
- Kodowanie wychodzących nagłówków HTTP.

Koder niestandardowy można utworzyć, korzystając z nowego typu *System. Web. util. HttpEncoder* , a następnie konfigurując ASP.NET do używania typu niestandardowego w sekcji *httpRuntime* pliku `Web.config`, jak pokazano w następującym przykładzie:

[!code-xml[Main](overview/samples/sample15.xml)]

Po skonfigurowaniu kodera niestandardowego program ASP.NET automatycznie wywołuje implementację niestandardowego kodowania przy każdym wywołaniu metod kodowania Public klasy *System. Web. HttpUtility* lub *System. Web. HttpServerUtility* . Pozwala to jednej części zespołu projektowego sieci Web utworzyć niestandardowy koder implementujący agresywne kodowanie znaków, podczas gdy pozostała część zespołu projektowego sieci Web nadal używa publicznych interfejsów API ASP.NET. Przez centralne skonfigurowanie kodera niestandardowego w elemencie *httpRuntime* , masz gwarancję, że wszystkie wywołania kodowania tekstu z publicznych interfejsów API ASP.NET kodowania są kierowane za pośrednictwem kodera niestandardowego.

<a id="0.2__Toc253429248"></a><a id="0.2__Toc243304622"></a>

### <a name="performance-monitoring-for-individual-applications-in-a-single-worker-process"></a>Monitorowanie wydajności dla poszczególnych aplikacji w pojedynczym procesie roboczym

W celu zwiększenia liczby witryn sieci Web, które mogą być hostowane na jednym serwerze, wielu hostów dostawcy uruchamia wiele aplikacji ASP.NET w jednym procesie roboczym. Jeśli jednak wiele aplikacji korzysta z jednego udostępnionego procesu roboczego, Administratorzy serwera trudno będzie zidentyfikować poszczególne aplikacje, na których występują problemy.

ASP.NET 4 wykorzystuje nowe funkcje monitorowania zasobów wprowadzone przez środowisko CLR. Aby włączyć tę funkcję, można dodać następujący fragment konfiguracji XML do pliku konfiguracji `aspnet.config`.

[!code-xml[Main](overview/samples/sample16.xml)]

> [!NOTE]
> Zwróć uwagę, że plik `aspnet.config` znajduje się w katalogu, w którym zainstalowano .NET Framework. Nie jest to plik `Web.config`.

Po włączeniu funkcji *appDomainResourceMonitoring* dostępne są dwa nowe liczniki wydajności w kategorii wydajności "aplikacje ASP.NET": *% czasu procesora zarządzanego* i *używanej pamięci zarządzanej*. Oba te liczniki wydajności używają nowej funkcji zarządzania zasobami aplikacji środowiska CLR do śledzenia szacowanego czasu procesora i wykorzystania pamięci zarządzanej przez poszczególne aplikacje ASP.NET. W związku z ASP.NET 4 Administratorzy mają teraz bardziej szczegółowy wgląd w użycie zasobów przez poszczególne aplikacje działające w pojedynczym procesie roboczym.

<a id="0.2__Toc253429249"></a><a id="0.2__Toc243304623"></a>

### <a name="multi-targeting"></a>Wielowersyjność kodu

Można utworzyć aplikację, która jest przeznaczona dla określonej wersji .NET Framework. W ASP.NET 4 nowy atrybut w elemencie *prekompilowania* pliku `Web.config` umożliwia kierowanie do .NET Framework 4 i nowszych. W przypadku jawnego określania .NET Framework 4 i w przypadku uwzględnienia opcjonalnych elementów w pliku `Web.config`, takich jak wpisy dla elementu *System. CodeDom*, te elementy muszą być poprawne dla .NET Framework 4. (Jeśli nie jest jawnie przeznaczony dla .NET Framework 4, platforma docelowa zostanie wywnioskowana z braku wpisu w pliku `Web.config`.)

W poniższym przykładzie pokazano użycie atrybutu *TargetFramework* w elemencie *compilation* pliku `Web.config`.

[!code-xml[Main](overview/samples/sample17.xml)]

Należy zwrócić uwagę na następujące informacje dotyczące określania konkretnej wersji .NET Framework:

- W puli aplikacji .NET Framework 4 system kompilacji ASP.NET zakłada, że .NET Framework 4 jako obiekt docelowy, jeśli plik `Web.config` nie zawiera atrybutu *TargetFramework* lub jeśli brakuje pliku `Web.config`. (Może zajść konieczność wprowadzenia zmian kodowania w aplikacji, aby była ona uruchamiana w .NET Framework 4).
- Jeśli dołączysz atrybut *TargetFramework* , a jeśli element *System. codeDom* jest zdefiniowany w pliku `Web.config`, ten plik musi zawierać poprawne wpisy dla .NET Framework 4.
- Jeśli używasz *\_polecenia kompilatora ASPNET* do prekompilowania aplikacji (na przykład w środowisku kompilacji), musisz użyć poprawnej wersji poleceń *kompilatora\_ASPNET* dla platformy docelowej. Użyj kompilatora, który został dostarczony z .NET Framework 2,0 (%WINDIR%\Microsoft.NET\Framework\v2.0.50727) do kompilowania dla .NET Framework 3,5 i wcześniejszych wersji. Użyj kompilatora, który jest dostarczany z .NET Framework 4 do kompilowania aplikacji utworzonych przy użyciu tej platformy lub nowszych wersji.
- W czasie wykonywania kompilator korzysta z najnowszych zestawów struktury zainstalowanych na komputerze (i w związku z tym w GAC). Jeśli aktualizacja została wprowadzona w późniejszym czasie do platformy (na przykład w przypadku zainstalowania hipotetycznej wersji 4,1), będzie można używać funkcji w nowszej wersji struktury, nawet jeśli atrybut *TargetFramework* jest przeznaczony dla mniejszej wersji (na przykład 4,0). (Jednak w czasie projektowania w programie Visual Studio 2010 lub przy użyciu polecenia *aspnet\_Compiler* użycie nowszych funkcji platformy spowoduje błędy kompilatora).

<a id="0.2__Toc224729023"></a><a id="0.2__Toc253429250"></a><a id="0.2__Toc243304624"></a>

## <a name="ajax"></a>AJAX

<a id="0.2__Toc253429251"></a><a id="0.2__Toc243304625"></a>

### <a name="jquery-included-with-web-forms-and-mvc"></a>jQuery dołączone do formularzy sieci Web i MVC

Szablony programu Visual Studio dla obu formularzy sieci Web i MVC obejmują bibliotekę jQuery Open Source. Podczas tworzenia nowej witryny sieci Web lub projektu tworzony jest folder skryptów zawierający następujące 3 pliki:

- jQuery-1.4.1. js — wersja unminified biblioteki jQuery, którą można odczytać przez człowieka.
- jQuery-14,1. min. js — wersja zminimalizowanego biblioteki jQuery.
- jQuery-1.4.1-vsdoc. js — plik dokumentacji IntelliSense dla biblioteki jQuery.

Uwzględnij wersję unminified platformy jQuery podczas tworzenia aplikacji. Uwzględnij wersję zminimalizowanego platformy jQuery dla aplikacji produkcyjnych.

Na przykład następująca strona formularzy sieci Web ilustruje, jak za pomocą platformy jQuery zmienić kolor tła kontrolek TextBox ASP.NET na żółty, gdy mają fokus.

[!code-aspx[Main](overview/samples/sample18.aspx)]

<a id="0.2__Toc253429252"></a><a id="0.2__Toc243304626"></a>

### <a name="content-delivery-network-support"></a>Obsługa Content Delivery Network

Microsoft Ajax Content Delivery Network (CDN) umożliwia łatwe dodawanie skryptów AJAX i jQuery do aplikacji sieci Web. Na przykład możesz zacząć korzystać z biblioteki jQuery po prostu dodając do strony tag `<script>`, który wskazuje na Ajax.microsoft.com w następujący sposób:

[!code-html[Main](overview/samples/sample19.html)]

Korzystając z zalet usługi Microsoft Ajax CDN, możesz znacząco poprawić wydajność aplikacji AJAX. Zawartość usługi Microsoft Ajax CDN jest buforowana na serwerach znajdujących się na całym świecie. Ponadto w usłudze Microsoft Ajax CDN przeglądarki mogą ponownie używać buforowanych plików JavaScript dla witryn sieci Web, które znajdują się w różnych domenach.

Content Delivery Network Microsoft Ajax obsługuje protokół SSL (HTTPS) w przypadku, gdy potrzebna jest strona sieci Web przy użyciu SSL.

Implementowanie rezerwy, gdy sieć CDN jest niedostępna. Przetestuj rezerwę.

Aby dowiedzieć się więcej na temat usługi Microsoft Ajax CDN, odwiedź następującą witrynę sieci Web:

[https://www.asp.net/ajaxlibrary/CDN.ashx](../../ajax/cdn/overview.md)

Element ScriptManager ASP.NET obsługuje usługę Microsoft Ajax CDN. Po prostu ustawiając jedną właściwość, właściwość EnableCdn można pobrać wszystkie pliki języka JavaScript środowiska ASP.NET Framework z sieci CDN:

[!code-aspx[Main](overview/samples/sample20.aspx)]

Po ustawieniu właściwości EnableCdn na wartość true, struktura ASP.NET będzie pobierać wszystkie pliki języka JavaScript środowiska ASP.NET Framework z usługi CDN, w tym wszystkie pliki JavaScript używane do walidacji i UpdatePanel. Ustawienie tej jednej właściwości może mieć znaczący wpływ na wydajność aplikacji sieci Web.

Ścieżkę sieci CDN dla własnych plików JavaScript można ustawić przy użyciu atrybutu WebResource. Nowa Właściwość CdnPath określa ścieżkę do sieci CDN używanej, gdy właściwość EnableCdn jest ustawiona na wartość true:

[!code-csharp[Main](overview/samples/sample21.cs)]

<a id="0.2__Toc253429253"></a><a id="0.2__Toc243304627"></a>

### <a name="scriptmanager-explicit-scripts"></a>Jawne skrypty ScriptManager

W przeszłości, jeśli użyto ASP.NET ScriptManger, wymagane jest załadowanie całej biblioteki lity ASP.NET AJAX. Wykorzystując nową właściwość ScriptManager. AjaxFrameworkMode, można kontrolować, które składniki biblioteki ASP.NET AJAX są ładowane i ładować tylko składniki biblioteki ASP.NET AJAX, które są potrzebne.

Właściwość ScriptManager. AjaxFrameworkMode można ustawić na następujące wartości:

- Włączone — określa, że kontrolka ScriptManager automatycznie zawiera plik skryptu MicrosoftAjax. js, który jest połączonym plikiem skryptu każdego podstawowego skryptu programu (starsze zachowanie).
- Wyłączone — określa, że wszystkie funkcje skryptów Microsoft Ajax są wyłączone oraz że formant ScriptManager nie odwołuje się automatycznie do żadnego skryptu.
- Explicit--określa, że jawnie zostaną dołączone odwołania do skryptu do poszczególnych plików skryptu programu Framework Core, których wymaga Strona, oraz że zostaną uwzględnione odwołania do zależności wymaganych przez każdy plik skryptu.

Na przykład, jeśli właściwość AjaxFrameworkMode jest ustawiona na wartość Explicit, można określić konkretne wymagane skrypty składnika ASP.NET AJAX:

[!code-aspx[Main](overview/samples/sample22.aspx)]

<a id="0.2__The_DataView_Control"></a><a id="0.2__The_DataContext_and"></a><a id="0.2__Refactoring_the_Microsoft"></a><a id="0.2__Toc224729032"></a><a id="0.2__Toc253429256"></a><a id="0.2__Toc243304630"></a>

## <a name="web-forms"></a>Formularze sieci Web

Formularze sieci Web są podstawową funkcją w ASP.NET od momentu wydania ASP.NET 1,0. W tym obszarze wprowadzono wiele ulepszeń dla ASP.NET 4, w tym następujące:

- Możliwość ustawiania tagów *meta* .
- Większa kontrola nad stanem widoku.
- Łatwiejsze sposoby pracy z funkcjami przeglądarki.
- Obsługa używania routingu ASP.NET z formularzami internetowymi.
- Większa kontrola nad wygenerowanymi identyfikatorami.
- Możliwość utrwalania wybranych wierszy w kontrolkach danych.
- Większa kontrola nad renderowanym kodem HTML w kontrolkach *FormView* i *ListView* .
- Obsługa filtrowania dla formantów źródła danych.

<a id="0.2__Toc224729033"></a><a id="0.2__Toc253429257"></a><a id="0.2__Toc243304631"></a>

### <a name="setting-meta-tags-with-the-pagemetakeywords-and-pagemetadescription-properties"></a>Ustawianie tagów Meta przy użyciu właściwości Page. metas i Page. dbdescription

ASP.NET 4 dodaje dwie właściwości do klasy *Page* , *sqlkeywords* i *dbdescription*. Te dwie właściwości reprezentują odpowiednie *meta* Tagi na stronie, jak pokazano w następującym przykładzie:

[!code-aspx[Main](overview/samples/sample23.aspx)]

Te dwie właściwości działają tak samo, jak Właściwość *title* strony. Są one zgodne z następującymi regułami:

1. Jeśli w elemencie *nagłówkowym* nie ma tagów *meta* , które pasują do nazw właściwości (to znaczy, Name = "keywords" dla *strony.* metadane i nazwa = "Description" dla *strony. dbdescription*, co oznacza, że te właściwości nie zostały ustawione), Tagi *meta* zostaną dodane do strony, gdy jest renderowany.
2. Jeśli z tymi nazwami istnieją już Tagi *meta* , te właściwości działają jako metody get i Set dla zawartości istniejących tagów.

Można ustawić te właściwości w czasie wykonywania, co pozwala uzyskać zawartość z bazy danych lub innego źródła, co pozwala na dynamiczne ustawianie tagów w celu opisania określonej strony.

Możesz również ustawić właściwości *Keywords* i *Description* w dyrektywie *@ Page* w górnej części znacznika strony formularzy sieci Web, jak w poniższym przykładzie:

[!code-aspx[Main](overview/samples/sample24.aspx)]

Spowoduje to zastąpienie zawartości znacznika *meta* (jeśli istnieje) już zadeklarowanej na stronie.

Zawartość taga *meta* Description służy do ulepszania podglądów listy wyszukiwania w usłudze Google. (Aby uzyskać szczegółowe informacje, zobacz artykuł [ulepszanie fragmentów kodu z przerabianiaem opisowym](https://googlewebmastercentral.blogspot.com/2007/09/improve-snippets-with-meta-description.html) w blogu Google Webmaster). Usługa Google i usługa Windows Live Search nie używają zawartości słów kluczowych dla wszystkiego, ale mogą to być inne wyszukiwarki. Aby uzyskać więcej informacji, zobacz [porady meta Keywords](http://www.searchengineguide.com/richard-ball/meta-keywords-a.php) w witrynie sieci Web przewodnika po aparacie wyszukiwania.

Nowe właściwości są prostą funkcją, ale zapiszą Cię z wymogu, aby dodać je ręcznie lub pisząc własny kod w celu utworzenia tagów *meta* .

<a id="0.2__Toc224729034"></a><a id="0.2__Toc253429258"></a><a id="0.2__Toc243304632"></a>

### <a name="enabling-view-state-for-individual-controls"></a>Włączanie stanu widoku dla poszczególnych kontrolek

Domyślnie stan widoku jest włączony dla strony, z wynikiem, że każda kontrolka na stronie potencjalnie przechowuje stan widoku, nawet jeśli nie jest to wymagane dla aplikacji. Dane stanu widoku są uwzględniane w znacznikach generowanych przez stronę i zwiększają ilość czasu potrzebną do wysłania strony klienta i jej opublikowania. Przechowywanie większej ilości widoku niż jest to konieczne może spowodować znaczny spadek wydajności. We wcześniejszych wersjach programu ASP.NET deweloperzy mogą wyłączyć stan widoku dla poszczególnych kontrolek w celu zmniejszenia rozmiaru strony, ale wymagało to jawnie dla poszczególnych kontrolek. W ASP.NET 4 formanty serwera sieci Web zawierają Właściwość *ViewStateMode* , która pozwala na wyłączenie stanu widoku domyślnie, a następnie włączenie go tylko dla kontrolek, które go wymagają na stronie.

Właściwość *ViewStateMode* przyjmuje Wyliczenie, które ma trzy wartości: *włączone*, *wyłączone*i *dziedziczenia*. *Włączone* włącza stan widoku dla tej kontrolki i dla wszystkich kontrolek podrzędnych, które są ustawione na *dziedziczenie* lub które nie mają ustawionych elementów. Wartość *wyłączone* powoduje wyłączenie stanu widoku i *dziedziczenie* określa, że kontrolka używa ustawienia *ViewStateMode* z kontrolki nadrzędnej.

Poniższy przykład pokazuje, jak działa Właściwość *ViewStateMode* . Znaczniki i kod dla kontrolek na poniższej stronie zawierają wartości właściwości *ViewStateMode* :

[!code-aspx[Main](overview/samples/sample25.aspx)]

Jak widać, kod wyłącza stan widoku dla kontrolki PlaceHolder1. Podrzędna kontrolka Label1 dziedziczy tę wartość właściwości (*dziedziczenie* jest wartością domyślną dla elementu *ViewStateMode* dla formantów) i w związku z tym nie zapisuje stanu widoku. W kontrolce PlaceHolder2, *ViewStateMode* ma wartość *Enabled*, więc etykiety 2 dziedziczy tę właściwość i zapisuje stan widoku. Po pierwszym załadowaniu strony Właściwość *Text* obu kontrolek *Label* jest ustawiona na ciąg "[wartość dynamiczną]".

Efekt tych ustawień polega na tym, że po pierwszym załadowaniu strony w przeglądarce wyświetlane są następujące dane wyjściowe:

`: [DynamicValue]` wyłączone

Włączone:`[DynamicValue]`

Po odświeżeniu zwrotnego są wyświetlane następujące dane wyjściowe:

`: [DeclaredValue]` wyłączone

Włączone:`[DynamicValue]`

Formant Label1 (którego wartość *ViewStateMode* jest ustawiona na *Disabled*) nie zachowa wartości, która została ustawiona w kodzie. Jednak formant etykiety 2 (którego wartość *ViewStateMode* jest ustawiona na *Enabled*) zachował swój stan.

Można również ustawić wartość *ViewStateMode* w dyrektywie *@ Page* , jak w poniższym przykładzie:

[!code-aspx[Main](overview/samples/sample26.aspx)]

Klasa *strony* jest tylko kolejną kontrolką; działa jako formant nadrzędny dla wszystkich innych kontrolek na stronie. Wartość domyślna elementu *ViewStateMode* jest *włączona* dla wystąpień *strony*. Ponieważ formanty domyślnie *dziedziczą*, formanty będą dziedziczyć wartość właściwości *Enabled* , chyba że ustawisz właściwość *ViewStateMode* na poziomie strony lub formantu.

Wartość właściwości *ViewStateMode* określa, czy stan widoku jest obsługiwany tylko wtedy, gdy właściwość *EnableViewState* ma *wartość true*. Jeśli właściwość *EnableViewState* ma wartość *false*, stan widoku nie zostanie zachowany, nawet jeśli ustawienie *ViewStateMode* ma wartość *włączone*.

Dobrym zastosowaniem tej funkcji jest z kontrolkami elementów *ContentPlaceHolder* na stronach wzorcowych, w których można ustawić właściwość *ViewStateMode* na wartość *Disabled* dla strony wzorcowej, a następnie włączyć ją indywidualnie dla kontrolek *ContentPlaceHolder* , które z kolei zawierają kontrolki wymagające stanu widoku.

<a id="0.2__Toc224729035"></a><a id="0.2__Toc253429259"></a><a id="0.2__Toc243304633"></a>

### <a name="changes-to-browser-capabilities"></a>Zmiany w możliwościach przeglądarki

ASP.NET określa możliwości przeglądarki używane przez użytkownika do przeglądania witryny przy użyciu funkcji o nazwie *możliwości przeglądarki*. Możliwości przeglądarki są reprezentowane przez obiekt *HttpBrowserCapabilities* (uwidoczniony przez właściwość *Request. Browser* ). Na przykład można użyć obiektu *HttpBrowserCapabilities* , aby określić, czy typ i wersja bieżącej przeglądarki obsługują określoną wersję języka JavaScript. Lub można użyć obiektu *HttpBrowserCapabilities* , aby określić, czy żądanie pochodzi z urządzenia przenośnego.

Obiekt *HttpBrowserCapabilities* jest oparty na zestawie plików definicji przeglądarki. Te pliki zawierają informacje o możliwościach konkretnych przeglądarek. W ASP.NET 4 pliki definicji przeglądarki zostały zaktualizowane, aby zawierały informacje o ostatnio wprowadzonych przeglądarkach i urządzeniach, takich jak Google Chrome, Research in Motion BlackBerry smartphone i Apple iPhone.

Na poniższej liście przedstawiono nowe pliki definicji przeglądarki:

- *BlackBerry. Browser*
- *Chrome. Browser*
- *Domyślna przeglądarka*
- *Firefox. Browser*
- *Brama. przeglądarka*
- *ogólna przeglądarka*
- *IE. Browser*
- *Iemobile. Browser*
- *iPhone. Browser*
- *Opera. Browser*
- *Przeglądarka Safari. Browser*

#### <a name="using-browser-capabilities-providers"></a>Korzystanie z dostawców możliwości przeglądarki

W programie ASP.NET w wersji 3,5 Service Pack 1 można zdefiniować możliwości dostępne w przeglądarce w następujący sposób:

- Na poziomie komputera można utworzyć lub zaktualizować `.browser` plik XML w następującym folderze:

- [!code-console[Main](overview/samples/sample27.cmd)]

- Po zdefiniowaniu możliwości przeglądarki należy uruchomić następujące polecenie w wierszu polecenia programu Visual Studio w celu odbudowania zestawu możliwości przeglądarki i dodania go do pamięci podręcznej:

- [!code-console[Main](overview/samples/sample28.cmd)]

- W przypadku pojedynczej aplikacji utworzysz plik `.browser` w folderze `App_Browsers` aplikacji.

Te podejścia wymagają zmiany plików XML, a w przypadku zmian na poziomie komputera należy ponownie uruchomić aplikację po uruchomieniu programu ASPNET\_RegBrowsers. exe.

ASP.NET 4 zawiera funkcję nazywaną *dostawcami możliwości przeglądarki*. Jak sugeruje nazwa, umożliwia to utworzenie dostawcy, który z kolei umożliwia użycie własnego kodu do określenia możliwości przeglądarki.

W tym przypadku deweloperzy często nie definiują możliwości niestandardowej przeglądarki. Pliki przeglądarki są trudne do zaktualizowania, proces aktualizacji jest dość skomplikowany, a składnia XML dla plików `.browser` może być złożona do użycia i zdefiniowania. Ten proces jest znacznie łatwiejszy w przypadku istnienia typowej składni definicji przeglądarki lub bazy danych, która zawierała aktualne definicje przeglądarki, a nawet usługa sieci Web dla tej bazy danych. Nowe funkcje usługodawców przeglądarki zapewniają, że te scenariusze są możliwe i praktyczne dla deweloperów innych firm.

Istnieją dwa główne podejścia do korzystania z nowej funkcji dostawcy możliwości przeglądarki ASP.NET 4: rozszerzanie funkcji definicji możliwości przeglądarki ASP.NET lub całkowite zastąpienie. W poniższych sekcjach opisano najpierw sposób zastąpienia funkcji, a następnie sposób jej rozbudowania.

#### <a name="replacing-the-aspnet-browser-capabilities-functionality"></a>Zastępowanie funkcji możliwości przeglądarki ASP.NET

Aby całkowicie zastąpić funkcję definicji możliwości przeglądarki ASP.NET, wykonaj następujące kroki:

1. Utwórz klasę dostawcy, która pochodzi z *HttpCapabilitiesProvider* i która zastępuje metodę *GetBrowserCapabilities* , jak w poniższym przykładzie: 

    [!code-csharp[Main](overview/samples/sample29.cs)]

    Kod w tym przykładzie tworzy nowy obiekt *HttpBrowserCapabilities* , określając tylko możliwości o nazwie Browser i ustawiając tę funkcję na MyCustomBrowser.
2. Zarejestruj dostawcę w aplikacji. 

    Aby można było użyć dostawcy z aplikacją, należy dodać atrybut *dostawcy* do sekcji *browserCaps* w plikach `Web.config` lub `Machine.config`. (Można również zdefiniować atrybuty dostawcy w elemencie *Location* dla określonych katalogów w aplikacji, na przykład w folderze dla określonego urządzenia przenośnego). Poniższy przykład pokazuje, jak ustawić atrybut *dostawcy* w pliku konfiguracji:

    [!code-xml[Main](overview/samples/sample30.xml)]

    Innym sposobem na zarejestrowanie nowej definicji możliwości przeglądarki jest użycie kodu, jak pokazano w poniższym przykładzie:

    [!code-csharp[Main](overview/samples/sample31.cs)]

    Ten kod musi działać w *aplikacji\_uruchomieniu* zdarzenia `Global.asax` pliku. Wszelkie zmiany klasy *BrowserCapabilitiesProvider* muszą wystąpić przed wykonaniem dowolnego kodu w aplikacji, aby upewnić się, że pamięć podręczna pozostaje w prawidłowym stanie dla rozpoznanego obiektu *HttpCapabilitiesBase* .

#### <a name="caching-the-httpbrowsercapabilities-object"></a>Buforowanie obiektu HttpBrowserCapabilities

W powyższym przykładzie występuje jeden problem, co oznacza, że kod będzie uruchamiany przy każdym wywołaniu niestandardowego dostawcy w celu uzyskania obiektu *HttpBrowserCapabilities* . Może to wystąpić wiele razy podczas każdego żądania. W przykładzie kod dla dostawcy nie robi więcej. Jeśli jednak kod w dostawcy niestandardowym wykonuje znaczną pracę w celu uzyskania obiektu *HttpBrowserCapabilities* , może to mieć wpływ na wydajność. Aby temu zapobiec, można buforować obiekt *HttpBrowserCapabilities* . Wykonaj następujące kroki:

1. Utwórz klasę, która pochodzi od *HttpCapabilitiesProvider*, podobnie jak w poniższym przykładzie: 

    [!code-csharp[Main](overview/samples/sample32.cs)]

    W przykładzie kod generuje klucz pamięci podręcznej przez wywołanie niestandardowej metody BuildCacheKey i pobiera czas buforowania przez wywołanie niestandardowej metody GetCacheTime. Kod następnie dodaje rozpoznany obiekt *HttpBrowserCapabilities* do pamięci podręcznej. Obiekt można pobrać z pamięci podręcznej i ponownie użyć w kolejnych żądaniach, które używają niestandardowego dostawcy.
2. Zarejestruj dostawcę w aplikacji zgodnie z opisem w poprzedniej procedurze.

#### <a name="extending-aspnet-browser-capabilities-functionality"></a>Rozszerzanie funkcji możliwości przeglądarki ASP.NET

W poprzedniej sekcji opisano sposób tworzenia nowego obiektu *HttpBrowserCapabilities* w ASP.NET 4. Możesz również zwiększyć funkcje możliwości przeglądarki ASP.NET, dodając nowe definicje możliwości przeglądarki do tych, które już znajdują się w ASP.NET. Można to zrobić bez użycia definicji przeglądarki XML. Poniższa procedura pokazuje, jak.

1. Utwórz klasę, która pochodzi od *HttpCapabilitiesEvaluator* i która zastępuje metodę *GetBrowserCapabilities* , jak pokazano w poniższym przykładzie: 

    [!code-csharp[Main](overview/samples/sample33.cs)]

    Ten kod najpierw używa funkcji możliwości przeglądarki ASP.NET, aby spróbować zidentyfikować przeglądarkę. Jeśli jednak żadna przeglądarka nie zostanie zidentyfikowana na podstawie informacji zdefiniowanych w żądaniu (oznacza to, że jeśli właściwość *przeglądarki* obiektu *HttpBrowserCapabilities* jest ciągiem "nieznany"), kod wywołuje dostawcę niestandardowego (MyBrowserCapabilitiesEvaluator) w celu zidentyfikowania przeglądarki.
2. Zarejestruj dostawcę w aplikacji zgodnie z opisem w poprzednim przykładzie.

#### <a name="extending-browser-capabilities-functionality-by-adding-new-capabilities-to-existing-capabilities-definitions"></a>Rozszerzanie funkcji możliwości przeglądarki przez dodawanie nowych funkcji do istniejących definicji możliwości

Oprócz tworzenia niestandardowego dostawcy definicji przeglądarki i dynamicznego tworzenia nowych definicji przeglądarki można rozciągnąć istniejące definicje przeglądarki z dodatkowymi możliwościami. Pozwala to korzystać z definicji zbliżonej do pożądanej, ale nie tylko kilku funkcji. W tym celu wykonaj poniższe kroki.

1. Utwórz klasę, która pochodzi od *HttpCapabilitiesEvaluator* i która zastępuje metodę *GetBrowserCapabilities* , jak pokazano w poniższym przykładzie: 

    [!code-csharp[Main](overview/samples/sample34.cs)]

    Przykładowy kod rozszerza istniejącą klasę ASP.NET *HttpCapabilitiesEvaluator* i pobiera obiekt *HttpBrowserCapabilities* , który jest zgodny z bieżącą definicją żądania, przy użyciu następującego kodu:

    [!code-csharp[Main](overview/samples/sample35.cs)]

    Kod może następnie dodać lub zmodyfikować możliwość dla tej przeglądarki. Istnieją dwa sposoby, aby określić nową funkcję przeglądarki:

    - Dodaj parę klucz/wartość do obiektu *IDictionary* , który jest udostępniany przez właściwość *Capabilities* obiektu *HttpCapabilitiesBase* . W poprzednim przykładzie kod dodaje funkcję o nazwie wielodotyku o wartości *true*.
    - Ustaw istniejące właściwości obiektu *HttpCapabilitiesBase* . W poprzednim przykładzie kod ustawia właściwość *Frames* na *true*. Ta właściwość jest po prostu metodą dostępu dla obiektu *IDictionary* , który jest udostępniany przez właściwość *Capabilities* . 

        > [!NOTE]
        > Zwróć uwagę, że ten model ma zastosowanie do dowolnej właściwości *HttpBrowserCapabilities*, w tym adapterów kontroli.
2. Zarejestruj dostawcę w aplikacji zgodnie z opisem w poprzedniej procedurze.

<a id="0.2__Toc224729036"></a><a id="0.2__Toc253429260"></a><a id="0.2__Toc243304634"></a>

### <a name="routing-in-aspnet-4"></a>Routing w ASP.NET 4

ASP.NET 4 dodaje wbudowaną obsługę routingu za pomocą formularzy sieci Web. Routing umożliwia skonfigurowanie aplikacji do akceptowania adresów URL żądań, które nie są mapowane na pliki fizyczne. Zamiast tego można użyć routingu do zdefiniowania adresów URL, które są zrozumiałe dla użytkowników i które mogą pomóc w optymalizacji aparatu wyszukiwania (wyszukiwarka) dla aplikacji. Na przykład adres URL strony zawierającej kategorie produktów w istniejącej aplikacji może wyglądać podobnie do poniższego przykładu:

[!code-console[Main](overview/samples/sample36.cmd)]

Za pomocą routingu można skonfigurować aplikację tak, aby akceptowała następujący adres URL w celu renderowania tych samych informacji:

[!code-console[Main](overview/samples/sample37.cmd)]

Routing jest dostępny od wersji ASP.NET 3,5 z dodatkiem SP1. (Aby zapoznać się z przykładem używania routingu w programie ASP.NET 3,5 z dodatkiem SP1, zobacz wpis [przy użyciu usługi Routing z WebForms](http://haacked.com/archive/2008/03/11/using-routing-with-webforms.aspx "Tytuł tego wpisu.") w blogu Krzysztof Haack). ASP.NET 4 zawiera jednak pewne funkcje, które ułatwiają korzystanie z routingu, w tym następujące:

- Klasa *PageRouteHandler* , która jest prostą obsługą protokołu HTTP, która jest używana podczas definiowania tras. Klasa przekazuje dane na stronę, do której jest kierowane żądanie.
- Nowe właściwości *HttpRequest. RequestContext* i *Page. RouteData* (który jest serwerem proxy dla obiektu *HttpRequest. RequestContext. RouteData* ). Te właściwości ułatwiają dostęp do informacji przesyłanych z trasy.
- Następujące nowe konstruktory wyrażeń, które są zdefiniowane w *System. Web. Build. RouteUrlExpressionBuilder* i *System. Web. Build. RouteValueExpressionBuilder*:
- *RouteUrl*, która zapewnia prosty sposób tworzenia adresu URL, który odpowiada adresowi URL trasy w formancie serwera ASP.NET.
- *RouteValue*, który zapewnia prosty sposób wyodrębnienia informacji z obiektu *RouteContext* .
- Klasa *RouteParameter* , która ułatwia przekazywanie danych zawartych w obiekcie *RouteContext* do zapytania dla kontrolki źródła danych (podobnie jak w przypadku [*FormParameter*](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx)).

#### <a name="routing-for-web-forms-pages"></a>Routing dla stron formularzy sieci Web

Poniższy przykład pokazuje, jak zdefiniować trasę formularzy sieci Web przy użyciu nowej metody *MapPageRoute* klasy *Route* :

[!code-csharp[Main](overview/samples/sample38.cs)]

ASP.NET 4 wprowadza metodę *MapPageRoute* . Poniższy przykład jest równoważny definicji SearchRoute pokazanej w poprzednim przykładzie, ale używa klasy *PageRouteHandler* .

[!code-csharp[Main](overview/samples/sample39.cs)]

Kod w przykładzie mapuje trasę na stronę fizyczną (w pierwszej trasie do `~/search.aspx`). Pierwsza definicja trasy określa również, że parametr o nazwie SearchTerm powinien zostać wyodrębniony z adresu URL i przekierowany do strony.

Metoda *MapPageRoute* obsługuje następujące przeciążenia metod:

- *MapPageRoute (String RouteName, String routeUrl, String physicalFile, bool checkPhysicalUrlAccess)*
- *MapPageRoute (String RouteName, String routeUrl, String physicalFile, bool checkPhysicalUrlAccess, RouteValueDictionary Defaults)*
- *MapPageRoute (String RouteName, String routeUrl, String physicalFile, bool checkPhysicalUrlAccess, RouteValueDictionary defaults, RouteValueDictionary ograniczenia)*

Parametr *checkPhysicalUrlAccess* określa, czy trasa ma sprawdzać uprawnienia zabezpieczeń do wysyłanej strony fizycznej (w tym przypadku wyszukiwania. aspx) oraz uprawnienia do PRZYCHODZĄCEGO adresu URL (w tym przypadku wyszukiwania/{SearchTerm}). Jeśli wartość *checkPhysicalUrlAccess* ma wartość *false*, sprawdzane są tylko uprawnienia przychodzącego adresu URL. Te uprawnienia są zdefiniowane w pliku `Web.config` przy użyciu następujących ustawień:

[!code-xml[Main](overview/samples/sample40.xml)]

W przykładowej konfiguracji dostęp jest zabroniony do `search.aspx` strony fizycznej dla wszystkich użytkowników z wyjątkiem tych, którzy znajdują się w roli administratora. Gdy parametr *checkPhysicalUrlAccess* ma wartość *true* (jest to wartość domyślna), tylko użytkownicy administracyjni mogą uzyskiwać dostęp do adresu URL/Search/{SearchTerm}, ponieważ Strona fizyczna Search. aspx jest ograniczona do użytkowników w tej roli. Jeśli *checkPhysicalUrlAccess* ma wartość *Fałsz* , a lokacja jest skonfigurowana tak, jak pokazano w poprzednim przykładzie, Wszyscy uwierzytelnieni użytkownicy mogą uzyskiwać dostęp do adresu URL/Search/{SearchTerm}.

#### <a name="reading-routing-information-in-a-web-forms-page"></a>Odczytywanie informacji o routingu na stronie formularzy sieci Web

W kodzie strony fizycznej formularzy sieci Web można uzyskać dostęp do informacji, które zostały wyodrębnione z adresu URL (lub innych informacji, które zostały dodane przez inny obiekt do obiektu *RouteData* ) przy użyciu dwóch nowych właściwości: *HttpRequest. RequestContext* i *Page. RouteData*. (*Page. RouteData* zawija. *RequestContext. RouteData*.) Poniższy przykład pokazuje, jak używać *Page. RouteData*.

[!code-csharp[Main](overview/samples/sample41.cs)]

Kod wyodrębnia wartość, która została przeniesiona do parametru SearchTerm, zgodnie z definicją w przykładowej trasie. Rozważ następujący adres URL żądania:

[!code-console[Main](overview/samples/sample42.cmd)]

Po wykonaniu tego żądania słowo "Scott" byłoby renderowane na stronie `search.aspx`.

#### <a name="accessing-routing-information-in-markup"></a>Uzyskiwanie dostępu do informacji routingu w znaczniku

Metoda opisana w poprzedniej sekcji pokazuje, jak uzyskać dane trasy w kodzie na stronie formularzy sieci Web. Można również używać wyrażeń w znacznikach, które dają dostęp do tych samych informacji. Konstruktory wyrażeń to zaawansowany i elegancki sposób pracy z kodem deklaratywnym. (Aby uzyskać więcej informacji, zapoznaj [się z wpisem](http://haacked.com/archive/2006/11/29/Express_Yourself_With_Custom_Expression_Builders.aspx) .

ASP.NET 4 zawiera dwa nowe konstruktory wyrażeń dla routingu formularzy sieci Web. Poniższy przykład pokazuje, jak z nich korzystać.

[!code-aspx[Main](overview/samples/sample43.aspx)]

W przykładzie wyrażenie *RouteUrl* służy do definiowania adresu URL opartego na parametrze trasy. Spowoduje to zapisanie, że chcesz, aby można było naliczać sobie pełny kod URL do oznaczenia i można później zmienić strukturę adresu URL bez konieczności wprowadzania jakichkolwiek zmian w tym łączu.

Na podstawie trasy zdefiniowanej wcześniej ten znacznik generuje następujący adres URL:

[!code-console[Main](overview/samples/sample44.cmd)]

ASP.NET automatycznie sprawdza poprawność trasy (oznacza to, że generuje prawidłowy adres URL) na podstawie parametrów wejściowych. W wyrażeniu można także uwzględnić nazwę trasy, która umożliwia określenie trasy do użycia.

Poniższy przykład pokazuje, jak używać wyrażenia *RouteValue* .

[!code-aspx[Main](overview/samples/sample45.aspx)]

Gdy strona zawierająca ten formant zostanie uruchomiona, w etykiecie zostanie wyświetlona wartość "Scott".

Wyrażenie *RouteValue* ułatwia korzystanie z danych tras w znacznikach i pozwala uniknąć konieczności pracy z bardziej skomplikowaną stroną. RouteData ["x"] składnia w znaczniku.

#### <a name="using-route-data-for-data-source-control-parameters"></a>Używanie danych tras dla parametrów kontroli źródła danych

Klasa *RouteParameter* pozwala określić dane trasy jako wartość parametru dla zapytań w kontrolce źródła danych. [Działa](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx) podobnie jak Klasa, jak pokazano w następującym przykładzie:

[!code-aspx[Main](overview/samples/sample46.aspx)]

W takim przypadku wartość parametru trasy SearchTerm zostanie użyta dla parametru @companyname w instrukcji *SELECT* .

<a id="0.2__Toc224729037"></a><a id="0.2__Toc253429261"></a><a id="0.2__Toc243304635"></a>

### <a name="setting-client-ids"></a>Ustawianie identyfikatorów klientów

Nowa właściwość *ClientIDMode* odnosi się do długiego problemu w ASP.NET, mianowicie, jak formanty tworzą atrybut *ID* dla elementów, które są przez nich renderowane. Znajomość atrybutu *ID* dla renderowanych elementów jest ważna, jeśli aplikacja zawiera skrypt klienta, który odwołuje się do tych elementów.

Atrybut *ID* w kodzie HTML, który jest renderowany dla kontrolek serwera sieci Web, jest generowany na podstawie właściwości *ClientID* formantu. Do ASP.NET 4 algorytm generowania atrybutu *ID* we właściwości *ClientID* ma na celu połączenie kontenera nazw (jeśli istnieje) z identyfikatorem i w przypadku powtórzonych kontrolek (jak w kontrolkach danych), aby dodać prefiks i numer sekwencyjny. Mimo że zawsze gwarantujemy, że identyfikatory kontrolek na stronie są unikatowe, algorytm spowodował nieprzewidywalne identyfikatory sterowania i dlatego trudno się odwołać w skrypcie klienta.

Nowa właściwość *ClientIDMode* pozwala na dokładniejsze określenie identyfikatora klienta dla kontrolek. Właściwość *ClientIDMode* można ustawić dla każdej kontrolki, w tym dla strony. Dostępne są następujące ustawienia:

- *AutoID* — jest to równoznaczne z algorytmem generowania wartości właściwości *ClientID* , które były używane we wcześniejszych wersjach ASP.NET.
- *Static* — określa, że wartość *ClientID* będzie taka sama jak identyfikator bez łączenia identyfikatorów nadrzędnych kontenerów nazw. Może to być przydatne w kontrolkach użytkownika sieci Web. Ponieważ kontrolka użytkownika sieci Web może znajdować się na różnych stronach i w różnych kontrolkach kontenera, może być trudno napisać skrypt klienta dla formantów, które używają algorytmu *AutoID* , ponieważ nie można przewidzieć wartości identyfikatora.
- *Przewidywalne* — ta opcja jest używana głównie do celów kontroli danych, które używają powtarzających się szablonów. Łączy właściwości identyfikatora kontenerów nazw kontrolki, ale wygenerowane wartości *ClientID* nie zawierają ciągów takich jak "ctlxxx". To ustawienie działa w połączeniu z właściwością *ClientIDRowSuffix* formantu. Właściwość *ClientIDRowSuffix* jest ustawiana na nazwę pola danych, a wartość tego pola jest używana jako sufiks dla wygenerowanej wartości *ClientID* . Zazwyczaj jako wartość *ClientIDRowSuffix* można użyć klucza podstawowego rekordu danych.
- *Dziedzicz* — to ustawienie jest domyślnym zachowaniem dla kontrolek. Określa, że generacja identyfikatora kontrolki jest taka sama jak jego element nadrzędny.

Właściwość *ClientIDMode* można ustawić na poziomie strony. Definiuje domyślną wartość *ClientIDMode* dla wszystkich kontrolek na bieżącej stronie.

Domyślną wartością *ClientIDMode* na poziomie strony jest *AutoID*, a domyślna wartość *ClientIDMode* na poziomie kontroli jest *dziedziczenia*. W związku z tym, jeśli ta właściwość nie zostanie ustawiona w dowolnym miejscu w kodzie, wszystkie kontrolki będą domyślnie zgodne z algorytmem *AutoID* .

Wartość na poziomie strony ustawia się w dyrektywie *@ Page* , jak pokazano w następującym przykładzie:

[!code-aspx[Main](overview/samples/sample47.aspx)]

Możesz również ustawić wartość *ClientIDMode* w pliku konfiguracji na poziomie komputera (komputera) lub na poziomie aplikacji. Definiuje to domyślne ustawienie *ClientIDMode* dla wszystkich kontrolek na wszystkich stronach w aplikacji. Jeśli ustawisz wartość na poziomie komputera, zdefiniuje domyślne ustawienie *ClientIDMode* dla wszystkich witryn sieci Web na tym komputerze. W poniższym przykładzie przedstawiono ustawienie *ClientIDMode* w pliku konfiguracji:

[!code-xml[Main](overview/samples/sample48.xml)]

Jak wspomniano wcześniej, wartość właściwości *ClientID* jest pochodną kontenera nazewnictwa dla obiektu nadrzędnego formantu. W niektórych scenariuszach, takich jak podczas korzystania ze stron wzorcowych, formanty mogą kończyć się identyfikatorami, takimi jak te, w następującym renderowanym kodzie HTML:

[!code-html[Main](overview/samples/sample49.html)]

Mimo że element *wejściowy* widoczny w znaczniku (z formantu *TextBox* ) ma tylko dwa kontenery nazewnictwa na stronie (zagnieżdżone kontrolki *ContentPlaceHolder* ), ze względu na sposób przetwarzania stron WZORCOWych, wynik końcowy jest identyfikatorem kontrolki podobnym do poniższego:

[!code-console[Main](overview/samples/sample50.cmd)]

Ten identyfikator powinien być unikatowy na stronie, ale jest niekonieczny do wykonania w większości celów. Załóżmy, że chcesz zmniejszyć długość renderowanego identyfikatora i mieć większą kontrolę nad sposobem generowania identyfikatora. (Na przykład chcesz wyeliminować prefiksy "ctlxxx"). Najprostszym sposobem osiągnięcia tego ustawienia jest ustawienie właściwości *ClientIDMode* , jak pokazano w następującym przykładzie:

[!code-aspx[Main](overview/samples/sample51.aspx)]

W tym przykładzie właściwość *ClientIDMode* jest ustawiona na wartość *static* dla zewnętrznego elementu *NamingPanel* i ma ustawioną wartość *przewidywalny* dla wewnętrznego elementu *NamingControl* . Te ustawienia powodują następujące oznakowanie (pozostała część strony, a na stronie wzorcowej założono, że jest to taka sama jak w poprzednim przykładzie):

[!code-html[Main](overview/samples/sample52.html)]

Ustawienie *statyczne* ma wpływ na Resetowanie hierarchii nazw dla wszystkich kontrolek wewnątrz zewnętrznego elementu *NamingPanel* i usunięcie identyfikatorów *ContentPlaceHolder* i *masterpage* z wygenerowanego identyfikatora. (Atrybut *nazwy* wyrenderowanych elementów jest nienaruszony, więc normalne funkcje ASP.NET są zachowywane w przypadku zdarzeń, wyświetlania stanu itd.) Efektem ubocznym resetowania hierarchii nazewnictwa jest to, że nawet jeśli przeniesiesz znaczniki dla elementów *NamingPanel* do innej kontrolki *ContentPlaceHolder* , renderowane identyfikatory klienta pozostają takie same.

> [!NOTE]
> Zwróć uwagę, że jest to konieczne, aby upewnić się, że renderowane identyfikatory formantów są unikatowe. Jeśli tak nie jest, może przerwać wszystkie funkcje, które wymagają unikatowych identyfikatorów dla poszczególnych elementów HTML, takich jak funkcja *Document. getElementById* .

#### <a name="creating-predictable-client-ids-in-data-bound-controls"></a>Tworzenie przewidywalnych identyfikatorów klientów w formantach powiązanych z danymi

Wartości *ClientID* , które są generowane dla kontrolek w kontrolce listy powiązanej z danymi przez starszy algorytm mogą być długie i nie są naprawdę przewidywalne. Funkcje *ClientIDMode* mogą pomóc mieć większą kontrolę nad sposobem generowania tych identyfikatorów.

Znaczniki w poniższym przykładzie zawierają formant *ListView* :

[!code-aspx[Main](overview/samples/sample53.aspx)]

W poprzednim przykładzie właściwości *ClientIDMode* i *RowClientIDRowSuffix* są ustawiane w znacznikach. Właściwość *ClientIDRowSuffix* może być używana tylko w formantach powiązanych z danymi, a jego zachowanie różni się w zależności od używanej kontrolki. Te różnice są następujące:

- Formant *GridView* — możesz określić nazwę jednej lub więcej kolumn w źródle danych, które są łączone w czasie wykonywania, aby utworzyć identyfikatory klientów. Na przykład jeśli ustawisz *RowClientIDRowSuffix* na "ProductName, ProductID", identyfikatory formantów dla renderowanych elementów będą mieć format podobny do następującego:

- [!code-console[Main](overview/samples/sample54.cmd)]

- *ListView* — formant — można określić pojedynczą kolumnę w źródle danych, która jest dołączana do identyfikatora klienta. Na przykład jeśli ustawisz *ClientIDRowSuffix* na "ProductName", renderowane identyfikatory formantów będą mieć format podobny do następującego:

- [!code-console[Main](overview/samples/sample55.cmd)]

- W takim przypadku końcowy 1 jest wyprowadzany z identyfikatora produktu bieżącego elementu danych.

- Formant *wzmacniania* — ten formant nie obsługuje właściwości *ClientIDRowSuffix* . W kontrolce *wzmacnia* indeks bieżącego wiersza. Jeśli używasz ClientIDMode = "przewidywalne" *z kontrolką* , generowane są identyfikatory klientów o następującym formacie:

- [!code-console[Main](overview/samples/sample56.cmd)]

- Końcowy 0 jest indeksem bieżącego wiersza.

Kontrolki *FormView* i *DetailsView* nie wyświetlają wielu wierszy, więc nie obsługują właściwości *ClientIDRowSuffix* .

<a id="0.2__Toc224729038"></a><a id="0.2__Toc253429262"></a><a id="0.2__Toc243304636"></a>

### <a name="persisting-row-selection-in-data-controls"></a>Utrwalanie wyboru wierszy w kontrolkach danych

Kontrolki *GridView* i *ListView* mogą pozwolić użytkownikom na wybranie wiersza. W poprzednich wersjach ASP.NET wybór został oparty na indeksie wierszy na stronie. Na przykład po wybraniu trzeciego elementu na stronie 1, a następnie przeniesieniu do strony 2 jest wybierany trzeci element na tej stronie.

Utrwalone zaznaczenie było początkowo obsługiwane tylko w projektach danych dynamicznych w .NET Framework 3,5 z dodatkiem SP1. Gdy ta funkcja jest włączona, bieżący wybrany element jest oparty na kluczu danych dla tego elementu. Oznacza to, że jeśli wybierzesz trzeci wiersz na stronie 1 i przejdziesz do strony 2, nic nie zostanie zaznaczone na stronie 2. Po przejściu z powrotem do strony 1 trzeci wiersz jest nadal zaznaczony. Utrwalone zaznaczenie jest teraz obsługiwane dla kontrolek *GridView* i *ListView* we wszystkich projektach przy użyciu właściwości *EnablePersistedSelection* , jak pokazano w następującym przykładzie:

[!code-aspx[Main](overview/samples/sample57.aspx)]

<a id="0.2__Toc253429263"></a><a id="0.2__Toc243304637"></a>

### <a name="aspnet-chart-control"></a>Kontrolka wykresu ASP.NET

Kontrolka *wykres* ASP.NET rozwija oferty wizualizacji danych w .NET Framework. Za pomocą kontrolki *Wykres* można tworzyć strony ASP.NET, które mają intuicyjne i wizualne atrakcyjne wykresy na potrzeby złożonej analizy statystycznej lub analitycznej. Formant *wykresu* ASP.NET został wprowadzony jako dodatek do .NET Framework wersji 3,5 SP1 i jest częścią wersji .NET Framework 4.

Kontrolka obejmuje następujące funkcje:

- 35 różne typy wykresów.
- Nieograniczona liczba obszarów wykresu, tytułów, legend i adnotacji.
- Szeroka gama ustawień wyglądu dla wszystkich elementów wykresu.
- Obsługa 3-D w przypadku większości typów wykresów.
- Inteligentne etykiety danych, które mogą być automatycznie dopasowane do punktów danych.
- Linie paskowe, podziały skali i skalowanie logarytmiczne.
- Ponad 50 formuł finansowych i statystycznych na potrzeby analizy i transformacji danych.
- Proste powiązania i manipulowanie danymi wykresu.
- Obsługa typowych formatów danych, takich jak daty, godziny i waluta.
- Obsługa dostosowywania międzydziałania i opartego na zdarzeniach, w tym zdarzeń dotyczących kliknięcia klienta przy użyciu technologii AJAX.
- Zarządzanie stanem.
- Binarne przesyłanie strumieniowe.

Poniższe ilustracje przedstawiają przykłady wykresów finansowych, które są tworzone przez formant wykresu ASP.NET.

<a id="0.2_graphic17"></a>![](overview/_static/image1.png)

Rysunek 2: przykłady kontrolki wykresu ASP.NET

Aby uzyskać więcej przykładów użycia kontrolki wykresu ASP.NET, Pobierz przykładowy kod na stronie [kontrolki wykresu dla programu Microsoft Chart](https://go.microsoft.com/fwlink/?LinkId=128300) w witrynie MSDN w sieci Web. Więcej przykładów zawartości społeczności można znaleźć na [forum formantów wykresów](https://go.microsoft.com/fwlink/?LinkId=128713).

#### <a name="adding-the-chart-control-to-an-aspnet-page"></a>Dodawanie kontrolki wykresu do strony ASP.NET

Poniższy przykład pokazuje, jak dodać formant *wykresu* do strony ASP.NET przy użyciu znaczników. W przykładzie formant *wykresu* tworzy wykres kolumnowy dla statycznych punktów danych.

[!code-aspx[Main](overview/samples/sample58.aspx)]

#### <a name="using-3-d-charts"></a>Korzystanie z wykresów trójwymiarowych

Kontrolka *Wykres* zawiera kolekcję *ChartAreas* , która może zawierać obiekty *obszar wykresu* , które definiują cechy obszarów wykresu. Na przykład aby użyć 3-D dla obszaru wykresu, należy użyć właściwości *Area3DStyle* , jak w poniższym przykładzie:

[!code-aspx[Main](overview/samples/sample59.aspx)]

Na poniższym rysunku przedstawiono wykres trójwymiarowy z czterema seriami typu wykresu *słupkowego* .

<a id="0.2_graphic18"></a>![](overview/_static/image2.png)

Rysunek 3:3-D wykres słupkowy

#### <a name="using-scale-breaks-and-logarithmic-scales"></a>Korzystanie z podziałów skali i skal logarytmicznych

Podziały skali i skale logarytmiczne są dwa dodatkowe sposoby dodawania złożoności do wykresu. Te funkcje są specyficzne dla każdej osi w obszarze wykresu. Na przykład, aby użyć tych funkcji na podstawowej osi Y obszaru wykresu, należy użyć właściwości *AxisY. Islogarytmiczne* i *ScaleBreakStyle* w obiekcie *obszar wykresu* . Poniższy fragment kodu pokazuje, jak używać podziałów skali na podstawowej osi Y.

[!code-aspx[Main](overview/samples/sample60.aspx)]

Na poniższym rysunku przedstawiono oś Y z włączonymi przerwami skalowania.

<a id="0.2_graphic19"></a>![](overview/_static/image3.png)

Ilustracja 4. skalowanie podziałów

<a id="0.2__QueryExtender"></a><a id="0.2__Toc224729041"></a><a id="0.2__Toc253429264"></a><a id="0.2__Toc243304638"></a>

### <a name="filtering-data-with-the-queryextender-control"></a>Filtrowanie danych za pomocą kontrolki QueryExtender

Bardzo typowe zadanie dla deweloperów tworzących strony sieci Web sterowane danymi polega na filtrowaniu danych. Jest to tradycyjnie wykonywane przez skompilowanie klauzul *WHERE* w kontrolkach źródła danych. Takie podejście może być skomplikowane, a w niektórych przypadkach składnia *gdzie* nie pozwala korzystać z pełnej funkcjonalności podstawowej bazy danych.

Aby ułatwić filtrowanie, dodano nową kontrolkę *QueryExtender* w ASP.NET 4. Ten formant można dodać do kontrolek *EntityDataSource* lub *LinqDataSource* w celu filtrowania danych zwracanych przez te kontrolki. Ponieważ kontrolka *QueryExtender* opiera się na LINQ, filtr jest stosowany na serwerze bazy danych przed wysłaniem danych na stronę, co skutkuje bardzo wydajnymi operacjami.

Kontrolka *QueryExtender* obsługuje wiele opcji filtrów. W poniższych sekcjach opisano te opcje i przedstawiono przykłady korzystania z nich.

#### <a name="search"></a>Wyszukiwanie

W przypadku opcji Search formant *QueryExtender* wykonuje wyszukiwanie w określonych polach. W poniższym przykładzie formant używa tekstu wprowadzonego w kontrolce TextBoxSearch i wyszukuje jego zawartość w `ProductName` i `Supplier.CompanyName` kolumny w danych, które są zwracane przez formant *LinqDataSource* .

[!code-aspx[Main](overview/samples/sample61.aspx)]

#### <a name="range"></a>Zakres

Opcja zakres jest podobna do opcji Search, ale określa parę wartości, aby zdefiniować zakres. W poniższym przykładzie formant *QueryExtender* przeszukuje kolumnę `UnitPrice` w danych zwróconych przez formant *LinqDataSource* . Zakres jest odczytywany z kontrolek TextBoxFrom i TextBoxTo na stronie.

[!code-aspx[Main](overview/samples/sample62.aspx)]

#### <a name="propertyexpression"></a>PropertyExpression

Opcja wyrażenia właściwości pozwala zdefiniować porównanie wartości właściwości. Jeśli wyrażenie zwróci *wartość true*, zwracane są testowane dane. W poniższym przykładzie formant *QueryExtender* filtruje dane przez porównanie danych w kolumnie `Discontinued` do wartości z kontrolki CheckBoxDiscontinued na stronie.

[!code-aspx[Main](overview/samples/sample63.aspx)]

#### <a name="customexpression"></a>CustomExpression

Na koniec możesz określić wyrażenie niestandardowe, które ma być używane z kontrolką *QueryExtender* . Ta opcja umożliwia wywołanie funkcji na stronie, która definiuje logikę filtru niestandardowego. Poniższy przykład pokazuje, jak deklaratywnie określić wyrażenie niestandardowe w kontrolce *QueryExtender* .

[!code-aspx[Main](overview/samples/sample64.aspx)]

Poniższy przykład pokazuje funkcję niestandardową, która jest wywoływana przez formant *QueryExtender* . W tym przypadku zamiast używać zapytania bazy danych zawierającego klauzulę *WHERE* , kod używa zapytania LINQ do filtrowania danych.

[!code-csharp[Main](overview/samples/sample65.cs)]

Te przykłady pokazują tylko jedno wyrażenie używane w kontrolce *QueryExtender* w danym momencie. Można jednak uwzględnić wiele wyrażeń wewnątrz kontrolki *QueryExtender* .

<a id="0.2__Toc253429265"></a><a id="0.2__Toc243304639"></a>

### <a name="html-encoded-code-expressions"></a>Wyrażenia kodu kodowane HTML

Niektóre lokacje ASP.NET (zwłaszcza z ASP.NET MVC) opierają się na użyciu składni `<%`= `expression %>` (często nazywanej "kodem nuggets") w celu zapisania tekstu w odpowiedzi. W przypadku używania wyrażeń kodowych można łatwo zapomnieć, aby kod HTML zakodować tekst, jeśli tekst pochodzi z danych wejściowych użytkownika, może pozostawiać strony otwarte do ataku typu XSS (cross-site scripting).

ASP.NET 4 wprowadza następującą nową składnię dla wyrażeń kodu:

[!code-aspx[Main](overview/samples/sample66.aspx)]

Ta składnia domyślnie używa kodowania HTML podczas zapisywania w odpowiedzi. To nowe wyrażenie skutecznie tłumaczy na następujące:

[!code-aspx[Main](overview/samples/sample67.aspx)]

Na przykład &lt;%: Request ["UserInput"]%&gt; wykonuje kodowanie HTML na wartość *żądania ["UserInput"]* .

Celem tej funkcji jest możliwość zastąpienia wszystkich wystąpień starej składni nową składnią, tak aby nie było wymuszane podejmowanie decyzji o każdym kroku, który ma być używany. Istnieją jednak przypadki, w których tekst danych wyjściowych jest przeznaczony do pliku HTML lub jest już zakodowany, w takim przypadku może to prowadzić do podwójnego kodowania.

W takich przypadkach ASP.NET 4 wprowadza nowy interfejs, *IHtmlString*, wraz z konkretną implementacją, *HtmlString*. Wystąpienia tych typów pozwalają wskazać, że wartość zwracana jest już poprawnie zakodowana (lub w inny sposób zbadana) do wyświetlania jako HTML, a w związku z tym wartość nie powinna być ponownie zakodowana w formacie HTML. Na przykład następujące elementy nie powinny być (i nie są) kodowane w formacie HTML:

[!code-aspx[Main](overview/samples/sample68.aspx)]

Metody pomocnika MVC 2 ASP.NET zostały zaktualizowane, aby współdziałać z tą nową składnią, tak aby nie były one kodowane podwójnie, ale tylko wtedy, gdy korzystasz z ASP.NET 4. Ta nowa składnia nie działa w przypadku uruchamiania aplikacji przy użyciu programu ASP.NET 3,5 z dodatkiem SP1.

Należy pamiętać, że nie gwarantuje to ochrony przed atakami XSS. Na przykład HTML, który używa wartości atrybutów, które nie są w cudzysłowie mogą zawierać dane wejściowe użytkownika, które są nadal podatne. Należy zauważyć, że dane wyjściowe formantów ASP.NET oraz pomocników ASP.NET MVC zawsze zawierają wartości atrybutów w cudzysłowie, które jest zalecanym podejściem.

Podobnie, składnia nie wykonuje kodowania JavaScript, na przykład podczas tworzenia ciągu JavaScript na podstawie danych wejściowych użytkownika.

<a id="0.2__Toc253429266"></a><a id="0.2__Toc243304640"></a>

### <a name="project-template-changes"></a>Zmiany szablonu projektu

W starszych wersjach programu ASP.NET, gdy program Visual Studio jest używany do tworzenia nowego projektu witryny sieci Web lub projektu aplikacji sieci Web, wynikowe projekty zawierają tylko domyślną stronę. aspx, domyślny plik `Web.config` i folder `App_Data`, jak pokazano na poniższej ilustracji:

<a id="0.2_graphic1A"></a>![](overview/_static/image4.png)

Program Visual Studio obsługuje również pusty typ projektu witryny sieci Web, który nie zawiera żadnych plików, jak pokazano na poniższym rysunku:

<a id="0.2_graphic1B"></a>![](overview/_static/image5.png)

W związku z tym na początku jest bardzo mało wskazówek na temat tworzenia produkcyjnej aplikacji sieci Web. W związku z tym ASP.NET 4 wprowadza trzy nowe szablony, jeden dla pustego projektu aplikacji sieci Web i drugi dla aplikacji sieci Web i projektu witryny sieci Web.

#### <a name="empty-web-application-template"></a>Pusty szablon aplikacji sieci Web

Jak sugeruje nazwa, pusty szablon aplikacji sieci Web jest to oddzielny projekt aplikacji sieci Web. Ten szablon projektu jest wybierany z okna dialogowego Nowy projekt programu Visual Studio, jak pokazano na poniższym rysunku:

[![](overview/_static/image7.png)](overview/_static/image6.png)

([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](overview/_static/image8.png))

Podczas tworzenia pustej aplikacji sieci Web ASP.NET program Visual Studio tworzy następujący układ folderu:

<a id="0.2_graphic1D"></a>![](overview/_static/image9.png)

Jest to podobne do układu pustej witryny sieci Web z wcześniejszych wersji programu ASP.NET z jednym wyjątkiem. W programie Visual Studio 2010 puste aplikacje sieci Web i puste projekty witryny sieci Web zawierają następujący minimalny `Web.config` pliku, który zawiera informacje używane przez program Visual Studio do identyfikowania struktury, do której odnosi się projekt:

<a id="0.2_graphic1E"></a>![](overview/_static/image10.png)

Bez tej właściwości *TargetFramework* , program Visual Studio domyślnie przekierowanie do .NET Framework 2,0 w celu zachowania zgodności podczas otwierania starszych aplikacji.

#### <a name="web-application-and-web-site-project-templates"></a>Szablony projektu aplikacji sieci Web i witryny sieci Web

Pozostałe dwa nowe szablony projektów, które są dostarczane z programem Visual Studio 2010, zawierają istotne zmiany. Na poniższej ilustracji przedstawiono układ projektu tworzony podczas tworzenia nowego projektu aplikacji sieci Web. (Układ projektu witryny sieci Web jest niemal identyczny).

- <a id="0.2_graphic1F"></a>![](overview/_static/image11.png)

Projekt zawiera kilka plików, które nie zostały utworzone we wcześniejszych wersjach. Ponadto nowy projekt aplikacji sieci Web jest skonfigurowany z podstawowymi funkcjami członkostwa, dzięki czemu można szybko rozpocząć Zabezpieczanie dostępu do nowej aplikacji. Ze względu na to, że plik `Web.config` dla nowego projektu zawiera wpisy, które są używane do konfigurowania członkostwa, ról i profilów. Poniższy przykład przedstawia plik `Web.config` dla nowego projektu aplikacji sieci Web. (W tym przypadku *rolamanager* jest wyłączona).

[![](overview/_static/image13.png)](overview/_static/image12.png)

([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](overview/_static/image14.png))

Projekt zawiera również drugi `Web.config` plik w katalogu `Account`. Drugi plik konfiguracyjny zapewnia sposób zabezpieczania dostępu do strony ChangePassword. aspx dla niezalogowanych użytkowników. Poniższy przykład pokazuje zawartość drugiego pliku `Web.config`.

![](overview/_static/image15.png)

Strony utworzone domyślnie w nowych szablonach projektów zawierają również większą zawartość niż w poprzednich wersjach. Projekt zawiera domyślną stronę wzorcową i plik CSS, a domyślna strona (default. aspx) jest skonfigurowana tak, aby domyślnie używać strony wzorcowej. W efekcie gdy aplikacja sieci Web lub witryna sieci Web jest uruchamiana po raz pierwszy, strona domyślna (Strona główna) jest już funkcjonalna. W rzeczywistości jest podobna do domyślnej strony widocznej podczas uruchamiania nowej aplikacji MVC.

[![](overview/_static/image17.png)](overview/_static/image16.png)

([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](overview/_static/image18.png))

Zamiarem tych zmian w szablonach projektu jest zapewnienie wskazówek dotyczących sposobu rozpoczęcia tworzenia nowej aplikacji sieci Web. Z semantycznie poprawną, ścisłe znaczniki zgodne z XHTML 1,0 i układem, który jest określony za pomocą CSS, strony w szablonach przedstawiają najlepsze rozwiązania dotyczące kompilowania aplikacji sieci Web ASP.NET 4. Strony domyślne mają również układ dwukolumnowy, który można łatwo dostosować.

Załóżmy na przykład, że dla nowej aplikacji sieci Web chcesz zmienić niektóre kolory i wstawić logo firmy zamiast logo aplikacji My ASP.NET. W tym celu należy utworzyć nowy katalog w obszarze `Content` do przechowywania obrazu logo:

<a id="0.2_graphic23"></a>![](overview/_static/image19.png)

Aby dodać obraz do strony, Otwórz plik `Site.Master`, Znajdź miejsce zdefiniowania tekstu aplikacji My ASP.NET i zastąp go elementem *Image* , którego atrybut *src* jest ustawiony na nowy obraz logo, jak w poniższym przykładzie:

[![](overview/_static/image21.png)](overview/_static/image20.png)

([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](overview/_static/image22.png))

Następnie można przejść do pliku site. CSS i zmodyfikować definicje klas CSS, aby zmienić kolor tła strony, a także nagłówek.

Wynikiem tych zmian jest możliwość wyświetlania dostosowanej strony głównej z bardzo małym nakładem pracy:

[![](overview/_static/image24.png)](overview/_static/image23.png)

([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](overview/_static/image25.png))

<a id="0.2__Toc253429267"></a><a id="0.2__Toc243304641"></a>

### <a name="css-improvements"></a>Udoskonalenia CSS

Jednym z głównych obszarów pracy w programie ASP.NET 4 jest ułatwienie renderowania kodu HTML zgodnego z najnowszymi standardami HTML. Obejmuje to zmiany w sposobie, w jaki formanty serwera sieci Web ASP.NET używają stylów CSS.

#### <a name="compatibility-setting-for-rendering"></a>Ustawienie zgodności dla renderowania

Domyślnie, gdy aplikacja sieci Web lub witryna sieci Web jest przeznaczona dla .NET Framework 4, atrybut *controlRenderingCompatibilityVersion* elementu *Pages* ma wartość "4,0". Ten element jest zdefiniowany w pliku `Web.config` na poziomie komputera i domyślnie dotyczy wszystkich aplikacji ASP.NET 4:

[!code-xml[Main](overview/samples/sample69.xml)]

Wartość dla *controlRenderingCompatibility* jest ciągiem, który umożliwia potencjalne nowe definicje wersji w przyszłych wydaniach. W bieżącej wersji dla tej właściwości obsługiwane są następujące wartości:

- "3.5". To ustawienie wskazuje na starsze renderowanie i adiustację. Znaczniki renderowane przez kontrolki są zgodne z poprzednimi wersjami (100%), a ustawienie właściwości *xhtmlConformance* jest honorowane.
- "4.0". Jeśli właściwość ma to ustawienie, ASP.NET kontroluje serwer sieci Web:
- Właściwość *xhtmlConformance* jest zawsze traktowana jako "Strict". W efekcie formanty renderują znaczniki języka XHTML 1,0 Strict.
- Wyłączenie formantów innych niż wejściowe nie powoduje już renderowania nieprawidłowych stylów.
- elementy *DIV* wokół ukrytych pól są teraz zgodne z stylami, więc nie zakłócają reguł CSS utworzonych przez użytkownika.
- Kontrolki menu renderuje znaczniki, które są semantycznie prawidłowe i są zgodne z wytycznymi dotyczącymi ułatwień dostępu.
- Kontrolki walidacji nie renderują wbudowanych stylów.
- Kontrolki, które wcześniej renderowane obramowanie = "0" (formanty, które pochodzą z formantu *tabeli* ASP.NET i formant *obrazu* ASP.NET), nie są już renderowane dla tego atrybutu.

#### <a name="disabling-controls"></a>Wyłączanie formantów

W programie ASP.NET 3,5 z dodatkiem SP1 i wcześniejszych wersji, struktura renderuje *wyłączony* atrybut w znaczniku HTML dla każdej kontrolki, której właściwość *Enabled* ma wartość *false*. Jednak zgodnie ze specyfikacją HTML 4,01, tylko elementy *wejściowe* powinny mieć ten atrybut.

W ASP.NET 4 można ustawić właściwość *controlRenderingCompatibilityVersion* na "3,5", jak w poniższym przykładzie:

[!code-xml[Main](overview/samples/sample70.xml)]

Można utworzyć znaczniki dla kontrolki *etykieta* podobne do następujących, która wyłącza formant:

[!code-aspx[Main](overview/samples/sample71.aspx)]

Kontrolka *etykieta* będzie renderować następujący kod HTML:

[!code-html[Main](overview/samples/sample72.html)]

W ASP.NET 4 można ustawić *controlRenderingCompatibilityVersion* na "4,0". W takim przypadku tylko kontrolki, które renderują elementy *wejściowe* , będą renderować *wyłączony* atrybut, gdy właściwość *Enabled* kontrolki ma wartość *Fałsz*. Kontrolki, które nie renderują elementów *wejściowych* HTML, zamiast tego Renderuj atrybut *klasy* , który odwołuje się do klasy CSS, której można użyć do zdefiniowania wyłączonego wyszukiwania dla kontrolki. Na przykład kontrolka *etykieta* wyświetlana w poprzednim przykładzie generuje następujący znacznik:

[!code-html[Main](overview/samples/sample73.html)]

Wartością domyślną dla klasy określonej dla tej kontrolki jest "aspNetDisabled". Można jednak zmienić tę wartość domyślną, ustawiając statyczną Właściwość static *DisabledCssClass* klasy *WebControl* . Dla deweloperów kontroli, zachowanie, które ma być używane dla konkretnej kontrolki, można również zdefiniować za pomocą właściwości *SupportsDisabledAttribute* .

<a id="0.2__Toc253429268"></a><a id="0.2__Toc243304642"></a>

### <a name="hiding-div-elements-around-hidden-fields"></a>Ukrywanie elementów div wokół ukrytych pól

ASP.NET 2,0 i nowsze wersje Renderuj specyficzne dla systemu pola ukryte (takie jak *ukryty* element używany do przechowywania informacji o stanie) wewnątrz elementu *DIV* w celu zapewnienia zgodności ze standardem XHTML. Jednak może to spowodować problem, gdy reguła CSS ma wpływ na elementy *DIV* na stronie. Na przykład może to spowodować, że na stronie dookoła ukrytych elementów *DIV* pojawia się jednopikselowa linia. W ASP.NET 4, *DIV* elementy otaczające ukryte pola wygenerowane przez ASP.NET Dodaj odwołanie do klasy CSS, jak w poniższym przykładzie:

[!code-html[Main](overview/samples/sample74.html)]

Następnie można zdefiniować klasę CSS, która ma zastosowanie tylko do elementów *ukrytych* , które są generowane przez ASP.NET, jak w poniższym przykładzie:

[!code-css[Main](overview/samples/sample75.css)]

<a id="0.2__Toc253429269"></a><a id="0.2__Toc243304643"></a>

### <a name="rendering-an-outer-table-for-templated-controls"></a>Renderowanie zewnętrznej tabeli dla kontrolek z szablonami

Domyślnie następujące kontrolki serwera sieci Web ASP.NET, które obsługują szablony są automatycznie opakowane w zewnętrzną tabelę, która jest używana do zastosowania stylów wbudowanych:

- *FormView*
- *Logowanie*
- *PasswordRecovery*
- *ChangePassword*
- *Kreatora*
- *Formancie CreateUserWizard*

Do tych kontrolek dodano nową właściwość o nazwie *RenderOuterTable* , która umożliwia usunięcie tabeli zewnętrznej z znaczników. Rozważmy na przykład poniższy przykład kontrolki *FormView* :

[!code-aspx[Main](overview/samples/sample76.aspx)]

Ten znacznik renderuje następujące dane wyjściowe na stronie, która zawiera tabelę HTML:

[!code-html[Main](overview/samples/sample77.html)]

Aby zapobiec renderowanym tabeli, można ustawić właściwość *RenderOuterTable* kontrolki *FormView* , jak w poniższym przykładzie:

[!code-aspx[Main](overview/samples/sample78.aspx)]

W poprzednim przykładzie pokazano następujące dane wyjściowe, bez elementów *Table*, *TR*i *TD* :

> Zawartość

To ulepszenie może ułatwić styl zawartości kontrolki za pomocą CSS, ponieważ żadne nieoczekiwane Tagi nie są renderowane przez formant.

> [!NOTE]
> Uwaga Ta zmiana wyłącza obsługę funkcji autoformat w projektancie programu Visual Studio 2010, ponieważ nie istnieje już element *tabeli* , który może hostować atrybuty stylu, które są generowane przez opcję autoformat.

<a id="0.2__Toc253429270"></a><a id="0.2__Toc243304644"></a>

### <a name="listview-control-enhancements"></a>Usprawnienia formantów ListView

Formant *ListView* został łatwiejszy do użycia w ASP.NET 4. Wcześniejsza wersja kontrolki wymaga określenia szablonu układu, który zawiera kontrolkę serwera o znanym IDENTYFIKATORze. W poniższej tabeli przedstawiono typowy przykład użycia formantu *ListView* w ASP.NET 3,5.

[!code-aspx[Main](overview/samples/sample79.aspx)]

W ASP.NET 4 formant *ListView* nie wymaga szablonu układu. Adiustację pokazaną w poprzednim przykładzie można zastąpić następującym znacznikiem:

[!code-aspx[Main](overview/samples/sample80.aspx)]

<a id="0.2__Toc253429271"></a><a id="0.2__Toc243304645"></a>

### <a name="checkboxlist-and-radiobuttonlist-control-enhancements"></a>Udoskonalenia formantów formant CheckBoxList i RadioButtonList

W programie ASP.NET 3,5 można określić układ dla *formant CheckBoxList* i *RadioButtonList* przy użyciu następujących dwóch ustawień:

- *Przepływ*. Formant *renderuje* elementy, aby zawierały jego zawartość.
- *Tabela*. Kontrolka renderuje element *tabeli* , aby zawierał jego zawartość.

Poniższy przykład pokazuje znaczniki dla każdej z tych kontrolek.

[!code-aspx[Main](overview/samples/sample81.aspx)]

Domyślnie formanty Renderuj HTML podobne do następujących:

[!code-html[Main](overview/samples/sample82.html)]

Ponieważ te kontrolki zawierają listy elementów, aby renderować semantykę kodu HTML, powinny renderować zawartość przy użyciu elementów HTML list (*li*). Ułatwia to użytkownikom odczytywanie stron sieci Web za pomocą technologii wspomagającej i ułatwia Stylowanie formantów przy użyciu CSS.

W ASP.NET 4 formanty *formant CheckBoxList* i *RadioButtonList* obsługują następujące nowe wartości właściwości *RepeatLayout* :

- *OrderedList* — zawartość jest renderowana jako *li* elementów w obrębie elementu *ol* .
- *Układy UnorderedList* — zawartość jest renderowana jako *li* elementów w obrębie elementu *ul* .

Poniższy przykład pokazuje, jak używać tych nowych wartości.

[!code-aspx[Main](overview/samples/sample83.aspx)]

Powyższy znacznik generuje następujący kod HTML:

[!code-html[Main](overview/samples/sample84.html)]

> [!NOTE]
> Uwaga Jeśli ustawisz *RepeatLayout* na *OrderedList* lub *układy UnorderedList*, właściwość *RepeatDirection* nie może być już używana i zgłosi wyjątek w czasie wykonywania, jeśli właściwość została ustawiona w znaczniku lub kodzie. Właściwość nie będzie miała żadnej wartości, ponieważ układ wizualizacji tych formantów jest zdefiniowany przy użyciu CSS.

<a id="0.2__Toc253429272"></a><a id="0.2__Toc243304646"></a>

### <a name="menu-control-improvements"></a>Ulepszenia kontrolek menu

Przed ASP.NET 4, formant *menu* renderuje serię tabel html. Sprawia to, że stosowanie stylów CSS poza ustawieniem właściwości wbudowanych i nie jest zgodne ze standardami dostępności.

W ASP.NET 4 kontrolka renderuje teraz HTML przy użyciu oznaczeń semantycznych, która składa się z listy nieuporządkowanej i elementów listy. Poniższy przykład przedstawia znaczniki na stronie ASP.NET dla formantu *menu* .

[!code-aspx[Main](overview/samples/sample85.aspx)]

Gdy strona renderuje, formant tworzy następujący kod HTML (kod *onkliknięcia* został pominięty dla jasności):

[!code-html[Main](overview/samples/sample86.html)]

Oprócz ulepszeń renderowania Nawigacja w menu została ulepszona przy użyciu funkcji zarządzania fokusem. Gdy formant *menu* staje się fokusem, możesz użyć klawiszy strzałek, aby nawigować po elementach. Kontrolka *menu* również dołącza dostępne zaawansowane role i atrybuty aplikacji internetowych (Aria) następującymi regułami[w](http://www.w3.org/TR/wai-aria-practices/#menu "Menu ARIA wytyczne")celu zwiększenia dostępności.

Style kontrolki menu są renderowane w bloku stylów w górnej części strony, a nie w wierszu z renderowanymi elementami HTML. Aby przejąć pełną kontrolę nad stylem kontrolki, można ustawić *wartość false*dla właściwości New *IncludeStyleBlock* , w takim przypadku blok style nie jest emitowany. Jednym ze sposobów korzystania z tej właściwości jest użycie funkcji autoformat w projektancie programu Visual Studio w celu ustawienia wyglądu menu. Następnie można uruchomić stronę, otworzyć źródło strony, a następnie skopiować renderowany blok stylu do zewnętrznego pliku CSS. W programie Visual Studio Cofnij style i ustaw *IncludeStyleBlock* na *false*. W wyniku tego wygląd menu jest definiowany przy użyciu stylów w zewnętrznym arkuszu stylów.

<a id="0.2__Toc253429273"></a><a id="0.2__Toc243304647"></a>

### <a name="wizard-and-createuserwizard-controls"></a>Kreator i kontrolki formancie CreateUserWizard

*Kreator* ASP.NET i kontrolki *formancie CreateUserWizard* obsługują szablony umożliwiające zdefiniowanie kodu HTML, który będzie renderowany. (*Formancie CreateUserWizard* pochodzi od *Kreatora*). W poniższym przykładzie pokazano znaczniki dla w pełni szablonowego formantu *formancie CreateUserWizard* :

[!code-aspx[Main](overview/samples/sample87.aspx)]

Kontrolka renderuje kod HTML podobny do poniższego:

[!code-html[Main](overview/samples/sample88.html)]

W programie ASP.NET 3,5 z dodatkiem SP1, chociaż można zmienić zawartość szablonu, nadal masz ograniczoną kontrolę nad danymi wyjściowymi kontrolki *Kreatora* . W ASP.NET 4 można utworzyć szablon *szablon LayoutTemplate* i Wstaw kontrolki *zastępcze* (przy użyciu nazw zarezerwowanych), aby określić, jak ma być renderowany *formant kreatora* . Poniższy przykład pokazuje:

[!code-aspx[Main](overview/samples/sample89.aspx)]

Przykład zawiera następujące nazwane symbole zastępcze w elemencie *szablon LayoutTemplate* :

- *headerPlaceholder* — w czasie wykonywania jest zastępowany przez zawartość elementu *HeaderTemplate* .
- *sideBarPlaceholder* — w czasie wykonywania jest zastępowany przez zawartość elementu *element SideBarTemplate* .
- *wizardStepPlaceHolder* — w czasie wykonywania jest zastępowany przez zawartość elementu *WizardStepTemplate* .
- *navigationPlaceholder* — w czasie wykonywania jest zastępowany przez zdefiniowane przez użytkownika Szablony nawigacji.

Znaczniki w przykładzie używające symboli zastępczych renderuje następujący kod HTML (bez zawartości zdefiniowanej w szablonach):

[!code-html[Main](overview/samples/sample90.html)]

Jedynym kodem HTML, który nie jest zdefiniowany przez użytkownika, jest element *span* . (Przewidujemy, że w przyszłych wersjach nawet element *span* nie będzie renderowany). Zapewnia to teraz pełną kontrolę nad praktycznie całą zawartością wygenerowaną przez formant *Kreatora* .

<a id="0.2_dyndata"></a><a id="0.2__Toc253429274"></a><a id="0.2__Toc243304648"></a><a id="0.2__Toc224729042"></a>

## <a name="aspnet-mvc"></a>ASP.NET MVC

ASP.NET MVC wprowadzono jako platformę dodatków do ASP.NET 3,5 SP1 w marcu 2009. Program Visual Studio 2010 zawiera ASP.NET MVC 2, który obejmuje nowe funkcje i możliwości.

<a id="0.2__Toc253429275"></a>

### <a name="areas-support"></a>Obsługa obszarów

Obszary umożliwiają grupowanie kontrolerów i widoków w sekcje dużej aplikacji w odizolowaniu względnym od innych sekcji. Każdy obszar można zaimplementować jako oddzielny projekt ASP.NET MVC, do którego może odwoływać się główna aplikacja. Ułatwia to zarządzanie złożonością podczas kompilowania dużej aplikacji i ułatwia wielu zespołom współdziałanie w ramach jednej aplikacji.

<a id="0.2__Toc253429276"></a>

### <a name="data-annotation-attribute-validation-support"></a>Obsługa walidacji atrybutów adnotacji z danymi

Atrybuty *DataAnnotations* umożliwiają dołączenie logiki walidacji do modelu przy użyciu atrybutów metadanych. Atrybuty *DataAnnotation* zostały wprowadzone w danych dynamicznych ASP.NET w ASP.NET 3,5 SP1. Te atrybuty zostały zintegrowane z domyślnym spinaczem modelu i zapewniają metodę opartą na metadanych do weryfikacji danych wejściowych użytkownika.

<a id="0.2__Toc253429277"></a>

### <a name="templated-helpers"></a>Pomocnicy z szablonami

Pomocnicy z szablonami umożliwiają automatyczne kojarzenie szablonów edycji i wyświetlania z typami danych. Na przykład możesz użyć pomocnika szablonu, aby określić, że element interfejsu użytkownika selektora dat jest automatycznie renderowany dla wartości *System. DateTime* . Jest to podobne do szablonów pól w ASP.NET danych dynamicznych.

Metody pomocnika *HTML. EditorFor* i *HTML. DisplayFor* mają wbudowaną obsługę renderowania standardowych typów danych oraz obiektów złożonych z wieloma właściwościami. Umożliwiają również dostosowanie renderowania przez umożliwienie stosowania atrybutów adnotacji danych, takich jak *DisplayName* i *ScaffoldColumn* , do obiektu *ViewModel* .

Często chcesz dostosować dane wyjściowe z pomocników interfejsu użytkownika jeszcze bardziej i mieć pełną kontrolę nad tym, co zostało wygenerowane. Metody pomocnika *HTML. EditorFor* i *HTML. DisplayFor* obsługują tę metodę przy użyciu mechanizmu tworzenia szablonów, który umożliwia definiowanie zewnętrznych szablonów, które mogą przesłonić i kontrolować renderowane dane wyjściowe. Szablony mogą być renderowane osobno dla klasy.

<a id="0.2__Toc253429278"></a><a id="0.2__Toc243304649"></a>

## <a name="dynamic-data"></a>Dane dynamiczne

Dane dynamiczne zostały wprowadzone w wersji .NET Framework 3,5 z dodatkiem SP1 w połowie 2008. Ta funkcja udostępnia wiele ulepszeń dotyczących tworzenia aplikacji opartych na danych, w tym:

- Środowisko RAD umożliwiające szybkie tworzenie witryny sieci Web opartej na danych.
- Automatyczna walidacja oparta na ograniczeniach zdefiniowanych w modelu danych.
- Możliwość łatwego zmieniania znacznika wygenerowanego dla pól w kontrolkach *GridView* i *DetailsView* przy użyciu szablonów pól, które są częścią projektu danych dynamicznych.

> [!NOTE]
> Uwaga Aby uzyskać więcej informacji, zobacz [dokumentację dynamiczną danych](https://msdn.microsoft.com/library/cc488545.aspx) w bibliotece MSDN.

W przypadku ASP.NET 4 dane dynamiczne zostały ulepszone, aby zapewnić deweloperom jeszcze większą moc do szybkiego tworzenia witryn sieci Web opartych na danych.

<a id="0.2__Toc253429279"></a><a id="0.2__Toc243304650"></a>

### <a name="enabling-dynamic-data-for-existing-projects"></a>Włączanie danych dynamicznych dla istniejących projektów

Funkcje danych dynamicznych dostarczone w .NET Framework 3,5 z dodatkiem SP1 spowodowały nowe funkcje, takie jak następujące:

- Szablony pól — udostępniają szablony oparte na typach danych dla formantów powiązanych z danymi. Szablony pól zapewniają prostszy sposób dostosowywania wyglądu formantów danych niż przy użyciu pól szablonów dla każdego pola.
- Walidacja — dane dynamiczne umożliwiają korzystanie z atrybutów klas danych w celu określenia walidacji dla typowych scenariuszy, takich jak wymagane pola, Sprawdzanie zakresu, sprawdzanie typów, dopasowywanie wzorców przy użyciu wyrażeń regularnych i Walidacja niestandardowa. Sprawdzanie poprawności jest wymuszane przez kontrolki danych.

Jednak te funkcje miały następujące wymagania:

- Warstwa dostępu do danych musiała być oparta na Entity Framework lub LINQ to SQL.
- Jedyne kontrolki źródła danych obsługiwane przez te funkcje były kontrolkami *EntityDataSource* lub *formantem LinqDataSource* .
- Funkcje wymagały projektu sieci Web, który został utworzony przy użyciu szablonów danych dynamicznych lub dynamicznych jednostek danych w celu uzyskania wszystkich plików, które były wymagane do obsługi tej funkcji.

Głównym celem obsługi danych dynamicznych w programie ASP.NET 4 jest umożliwienie nowej funkcjonalności danych dynamicznych dla dowolnej aplikacji ASP.NET. W poniższym przykładzie przedstawiono znaczniki dla kontrolek, które mogą korzystać z funkcji danych dynamicznych na istniejącej stronie.

[!code-aspx[Main](overview/samples/sample91.aspx)]

W kodzie strony należy dodać następujący kod w celu włączenia obsługi dynamicznej danych dla tych formantów:

[!code-csharp[Main](overview/samples/sample92.cs)]

Gdy formant *GridView* jest w trybie edycji, dane dynamiczne automatycznie sprawdzają, czy wprowadzone dane mają prawidłowy format. Jeśli tak nie jest, zostanie wyświetlony komunikat o błędzie.

Ta funkcja zapewnia również inne korzyści, takie jak możliwość określania wartości domyślnych dla trybu wstawiania. Bez danych dynamicznych, aby zaimplementować wartość domyślną dla pola, należy dołączyć do zdarzenia, zlokalizować kontrolkę (przy użyciu *FindControl*) i ustawić jej wartość. W ASP.NET 4, wywołanie *EnableDynamicData* obsługuje drugi parametr, który umożliwia przekazanie wartości domyślnych dla dowolnego pola w obiekcie, jak pokazano w tym przykładzie:

[!code-csharp[Main](overview/samples/sample93.cs)]

<a id="0.2__Toc224729043"></a><a id="0.2__Toc253429280"></a><a id="0.2__Toc243304651"></a>

### <a name="declarative-dynamicdatamanager-control-syntax"></a>Deklaratywna składnia formantów Menedżera danych dynamicznych

Formant *Menedżera danych dynamicznych* został ulepszony, dzięki czemu można go skonfigurować deklaratywnie, tak jak w przypadku większości kontrolek w ASP.NET, a nie tylko w kodzie. Znaczniki dla kontrolki *Menedżera danych dynamicznych* wyglądają podobnie jak w poniższym przykładzie:

[!code-aspx[Main](overview/samples/sample94.aspx)]

Ten znacznik umożliwia dynamiczne zachowanie danych dla formantu GridView1, do którego odwołuje się sekcja *DataControls* formantu *Menedżera danych dynamicznych* .

<a id="0.2__Toc224729044"></a><a id="0.2__Toc253429281"></a><a id="0.2__Toc243304652"></a>

### <a name="entity-templates"></a>Szablony jednostek

Szablony jednostek oferują nowy sposób dostosowywania układu danych bez konieczności tworzenia niestandardowej strony. Szablony stron używają kontrolki *FormView* (zamiast kontrolki *DetailsView* , jak w szablonach stron we wcześniejszych wersjach danych dynamicznych) i kontrolki *DynamicControl* do renderowania szablonów jednostek. Daje to większą kontrolę nad adiustacją renderowaną przez dane dynamiczne.

Na poniższej liście przedstawiono nowy układ katalogu projektu, który zawiera szablony jednostek:

[!code-console[Main](overview/samples/sample95.cmd)]

Katalog `EntityTemplate` zawiera szablony służące do wyświetlania obiektów modelu danych. Domyślnie obiekty są renderowane przy użyciu szablonu `Default.ascx`, który zapewnia znaczniki, które wyglądają podobnie jak znaczniki utworzone przez formant *DetailsView* używany przez dane dynamiczne w ASP.NET 3,5 SP1. Poniższy przykład pokazuje znaczniki dla kontrolki `Default.ascx`:

[!code-aspx[Main](overview/samples/sample96.aspx)]

Szablony domyślne można edytować, aby zmienić wygląd i działanie całej witryny. Istnieją szablony służące do wyświetlania, edytowania i wstawiania operacji. Nowe szablony można dodać na podstawie nazwy obiektu danych, aby zmienić wygląd i działanie tylko jednego typu obiektu. Na przykład można dodać następujący szablon:

[!code-console[Main](overview/samples/sample97.cmd)]

Szablon może zawierać następujące znaczniki:

[!code-aspx[Main](overview/samples/sample98.aspx)]

Nowe szablony jednostek są wyświetlane na stronie przy użyciu nowej kontrolki *DynamicControl* . W czasie wykonywania ten formant jest zastępowany zawartością szablonu jednostki. Poniższe znaczniki przedstawiają formant *FormView* w `Detail.aspx` szablonie strony, który używa szablonu jednostki. Zwróć uwagę na element *DynamicControl* w znaczniku.

[!code-aspx[Main](overview/samples/sample99.aspx)]

<a id="0.2__Toc224729045"></a><a id="0.2__Toc253429282"></a><a id="0.2__Toc243304653"></a>

### <a name="new-field-templates-for-urls-and-email-addresses"></a>Nowe szablony pól dla adresów URL i adresów E-mail

ASP.NET 4 wprowadza dwa nowe wbudowane szablony pól, `EmailAddress.ascx` i `Url.ascx`. Te szablony są używane dla pól, które są oznaczone jako *EmailAddress* lub *URL* z atrybutem *DataType* . W przypadku obiektów *EmailAddress* pole jest wyświetlane jako hiperłącze, które jest tworzone przy użyciu protokołu *mailto:* . Po kliknięciu linku przez użytkownika zostanie otwarty klient poczty e-mail użytkownika i zostanie utworzony komunikat szkieletowy. Obiekty wpisywane jako *adresy URL* są wyświetlane jako zwykłe hiperłącza.

Poniższy przykład pokazuje, jak można oznaczyć pola.

[!code-csharp[Main](overview/samples/sample100.cs)]

<a id="0.2__Toc224729046"></a><a id="0.2__Toc253429283"></a><a id="0.2__Toc243304654"></a>

### <a name="creating-links-with-the-dynamichyperlink-control"></a>Tworzenie linków z kontrolką formantem DynamicHyperLink

Dane dynamiczne korzystają z nowej funkcji routingu dodanej w .NET Framework 3,5 programu SP1 w celu kontrolowania adresów URL, które użytkownicy końcowi widzą podczas uzyskiwania dostępu do witryny sieci Web. Nowy formant *formantem DynamicHyperLink* ułatwia tworzenie linków do stron w dynamicznej witrynie danych. Poniższy przykład pokazuje, jak używać kontrolki *formantem DynamicHyperLink* :

[!code-aspx[Main](overview/samples/sample101.aspx)]

Ten znacznik tworzy łącze wskazujące stronę listy dla tabeli `Products` w oparciu o trasy, które są zdefiniowane w pliku `Global.asax`. Kontrolka automatycznie używa domyślnej nazwy tabeli, na której opiera się Strona dane dynamiczne.

<a id="0.2__Toc224729047"></a><a id="0.2__Toc253429284"></a><a id="0.2__Toc243304655"></a>

### <a name="support-for-inheritance-in-the-data-model"></a>Obsługa dziedziczenia w modelu danych

Zarówno Entity Framework, jak i LINQ to SQL obsługują dziedziczenie w swoich modelach danych. Przykładem może być baza danych z tabelą `InsurancePolicy`. Może również zawierać `CarPolicy` i `HousePolicy` tabel, które mają takie same pola jak `InsurancePolicy`, a następnie dodać więcej pól. Dane dynamiczne zostały zmodyfikowane w celu zrozumienia dziedziczonych obiektów w modelu danych i obsługi szkieletów dla dziedziczonych tabel.

<a id="0.2__Toc224729048"></a><a id="0.2__Toc253429285"></a><a id="0.2__Toc243304656"></a>

### <a name="support-for-many-to-many-relationships-entity-framework-only"></a>Obsługa relacji wiele-do-wielu (tylko Entity Framework)

Entity Framework ma rozbudowaną obsługę relacji wiele-do-wielu między tabelami, które są implementowane przez ujawnienie relacji jako kolekcji w obiekcie *Entity* . Dodano nowe szablony pól `ManyToMany.ascx` i `ManyToMany_Edit.ascx`, aby zapewnić obsługę wyświetlania i edytowania danych, które są związane z relacjami wiele-do-wielu.

<a id="0.2__Toc224729049"></a><a id="0.2__Toc253429286"></a><a id="0.2__Toc243304657"></a>

### <a name="new-attributes-to-control-display-and-support-enumerations"></a>Nowe atrybuty do kontroli wyświetlania i obsługi wyliczeń

Został *dodany, aby* dać dodatkową kontrolę nad sposobem wyświetlania pól. Atrybut *DisplayName* we wcześniejszych wersjach danych dynamicznych umożliwiał zmianę nazwy, która jest używana jako napis dla pola. Nowa Klasa *inplayattribute* pozwala określić więcej opcji wyświetlania pola, takich jak kolejność, w której pole jest wyświetlane i czy pole będzie używane jako filtr. Ten atrybut zapewnia również niezależną kontrolę nazwy używanej dla etykiet w formancie *GridView* , nazwy używanej w kontrolce *DetailsView* , tekstu pomocy dla pola i znaku wodnego używanego dla pola (Jeśli pole przyjmuje tekst wejściowy).

Dodano klasę *EnumDataTypeAttribute* , aby umożliwić Mapowanie pól na wyliczenia. Po zastosowaniu tego atrybutu do pola należy określić typ wyliczeniowy. Dane dynamiczne używają nowego szablonu pola `Enumeration.ascx` do tworzenia interfejsu użytkownika do wyświetlania i edytowania wartości wyliczenia. Szablon mapuje wartości z bazy danych do nazw w wyliczeniu.

<a id="0.2__Toc224729050"></a><a id="0.2__Toc253429287"></a><a id="0.2__Toc243304658"></a>

### <a name="enhanced-support-for-filters"></a>Ulepszona obsługa filtrów

Dane dynamiczne 1,0 dostarczone z wbudowanymi filtrami dla kolumn logicznych i kolumn klucza obcego. Filtry nie pozwalają na określenie, czy były wyświetlane lub w jakiej kolejności. Nowy atrybut *playattribute* dotyczy obydwu tych problemów, dając kontrolę nad tym, czy kolumna jest wyświetlana jako filtr, i w jakiej kolejności będzie wyświetlana.

Dodatkowe Ulepszenie polega na tym, że obsługa filtrowania została[zaprojektowana w celu użycia nowej](#0.2__QueryExtender "_QueryExtender") funkcji formularzy sieci Web. Pozwala to na tworzenie filtrów bez znajomości kontroli źródła danych, z którą będą korzystać filtry. Wraz z tymi rozszerzeniami filtry zostały również włączone w kontrolkach szablonów, co pozwala na dodawanie nowych. Na *koniec, wymieniona wcześniej Klasa umożliwia* przesłanianie domyślnego filtru w taki sam sposób, w jaki *UIHint* zezwala na zastąpienie domyślnego szablonu pola kolumny.

<a id="0.2__Toc224729051"></a><a id="0.2__Toc253429288"></a><a id="0.2__Toc243304659"></a>

## <a name="visual-studio-2010-web-development-improvements"></a>Ulepszenia programu Visual Studio 2010 Web Development

Programowanie dla sieci Web w programie Visual Studio 2010 zostało ulepszone, aby zapewnić lepszą zgodność z CSS, zwiększoną produktywność dzięki ASP.NET kodu HTML i znaczników oraz nowym dynamicznym IntelliSense JavaScript.

<a id="0.2__Toc224729052"></a><a id="0.2__Toc253429289"></a><a id="0.2__Toc243304660"></a>

### <a name="improved-css-compatibility"></a>Lepsza zgodność stylów CSS

Program Visual Web Developer Designer w programie Visual Studio 2010 został zaktualizowany w celu usprawnienia zgodności ze standardami CSS 2,1. Projektant lepiej zachowuje integralność źródła HTML i jest bardziej niezawodna niż w poprzednich wersjach programu Visual Studio. W obszarze wystawcy wprowadzono również ulepszenia architektury, które lepiej umożliwią korzystanie z przyszłych ulepszeń w zakresie renderowania, układu i obsługi.

<a id="0.2__Toc224729053"></a><a id="0.2__Toc253429290"></a><a id="0.2__Toc243304661"></a>

### <a name="html-and-javascript-snippets"></a>Fragmenty kodu HTML i JavaScript

W edytorze HTML, funkcja IntelliSense uzupełnia nazwy tagów autouzupełniania. Fragmenty kodu IntelliSense wypełniają funkcję autouzupełniania całych tagów i nie tylko. W programie Visual Studio 2010 fragmenty kodu IntelliSense są obsługiwane dla języka JavaScript C# oraz Visual Basic, które były obsługiwane we wcześniejszych wersjach programu Visual Studio.

Program Visual Studio 2010 zawiera ponad 200 wstawek, które ułatwiają Autouzupełnianie typowych tagów ASP.NET i HTML, w tym wymaganych atrybutów (takich jak runat = "Server") i wspólnych atrybutów specyficznych dla tagu (takich jak *ID*, *DataSourceID*, *ControlToValidate obiektu*i *Text*).

Możesz pobrać dodatkowe fragmenty kodu lub napisać własne fragmenty kodu, które hermetyzują bloki znaczników, których ty lub Twój zespół używa do wykonywania typowych zadań.

<a id="0.2__Toc224729054"></a><a id="0.2__Toc253429291"></a><a id="0.2__Toc243304662"></a>

### <a name="javascript-intellisense-enhancements"></a>Ulepszenia funkcji IntelliSense języka JavaScript

W programie Visual 2010 funkcja IntelliSense języka JavaScript została przeprojektowana w celu zapewnienia jeszcze bogatszego środowiska edycji. Funkcja IntelliSense rozpoznaje teraz obiekty, które zostały dynamicznie wygenerowane przez metody, takie jak *registerNamespace* i podobne techniki używane przez inne struktury JavaScript. Ulepszono wydajność w celu analizowania dużych bibliotek skryptów i wyświetlania technologii IntelliSense z niewielkim opóźnieniem przetwarzania lub bez niego. Znacznie zwiększono zgodność w celu obsługi niemal wszystkich bibliotek innych firm i obsługi różnorodnych stylów kodowania. Komentarze do dokumentacji są teraz analizowane podczas pisania i są natychmiast wykorzystywane przez funkcję IntelliSense.

<a id="0.2__Toc224729055"></a><a id="0.2__Toc253429292"></a><a id="0.2__Toc243304663"></a>

## <a name="web-application-deployment-with-visual-studio-2010"></a>Wdrażanie aplikacji sieci Web za pomocą programu Visual Studio 2010

Gdy deweloperzy ASP.NET wdrażają aplikację sieci Web, często napotykają one następujące problemy:

- Wdrożenie w udostępnionej lokacji hostingu wymaga takich technologii jak FTP, co może potrwać wolno. Ponadto należy ręcznie wykonać zadania, takie jak uruchamianie skryptów SQL, aby skonfigurować bazę danych i należy zmienić ustawienia usług IIS, takie jak skonfigurowanie folderu katalogu wirtualnego jako aplikacji.
- W środowisku przedsiębiorstwa oprócz wdrażania plików aplikacji sieci Web Administratorzy często muszą modyfikować pliki konfiguracji ASP.NET i ustawienia usług IIS. Aby można było korzystać z bazy danych aplikacji, administratorzy baz danych muszą uruchamiać wiele skryptów SQL. Takie instalacje są pracochłonne, często trwają kilka godzin i muszą być starannie udokumentowane.

Program Visual Studio 2010 zawiera technologie pozwalające rozwiązać te problemy, dzięki czemu można bezproblemowo wdrażać aplikacje sieci Web. Jedną z tych technologii jest narzędzie do wdrażania w sieci Web usług IIS (MsDeploy. exe).

Funkcje wdrażania w sieci Web w programie Visual Studio 2010 obejmują następujące obszary główne:

- Pakiet internetowy
- Transformacja pliku Web. config
- Wdrażanie bazy danych
- Publikowanie aplikacji sieci Web jednym kliknięciem

Poniższe sekcje zawierają szczegółowe informacje o tych funkcjach.

<a id="0.2__Toc224729056"></a><a id="0.2__Toc253429293"></a><a id="0.2__Toc243304664"></a>

### <a name="web-packaging"></a>Pakiet internetowy

Program Visual Studio 2010 używa narzędzia MSDeploy do utworzenia skompresowanego pliku (. zip) dla aplikacji, która jest nazywana *pakietem internetowym*. Plik pakietu zawiera metadane dotyczące aplikacji oraz następującą zawartość:

- Ustawienia usług IIS, w tym ustawienia puli aplikacji, ustawienia strony błędów i tak dalej.
- Rzeczywista zawartość sieci Web, w tym strony sieci Web, kontrolki użytkownika, zawartość statyczna (obrazy i pliki HTML) itd.
- SQL Server schematy i dane bazy danych.
- Certyfikaty zabezpieczeń, składniki do zainstalowania w pamięci podręcznej GAC, ustawienia rejestru i tak dalej.

Pakiet internetowy można skopiować na dowolny serwer, a następnie zainstalować ręcznie przy użyciu Menedżera usług IIS. Alternatywnie, w celu automatycznego wdrażania, pakiet można zainstalować za pomocą poleceń wiersza polecenia lub interfejsów API wdrażania.

Program Visual Studio 2010 zawiera wbudowane zadania i elementy docelowe programu MSBuild służące do tworzenia pakietów internetowych. Aby uzyskać więcej informacji, zobacz temat [wdrażanie projektu aplikacji sieci web ASP.NET](https://msdn.microsoft.com/library/dd394698%28VS.100%29.aspx) w witrynie MSDN w sieci Web i [10 + 20 powodów, dla których należy utworzyć pakiet sieci Web](http://vishaljoshi.blogspot.com/2009/07/10-20-reasons-why-you-should-create-web.html) na blogu Vishal Joshi.

<a id="0.2__Toc224729057"></a><a id="0.2__Toc253429294"></a><a id="0.2__Toc243304665"></a>

### <a name="webconfig-transformation"></a>Transformacja pliku Web. config

W przypadku wdrażania aplikacji sieci Web program Visual Studio 2010 wprowadza [transformację dokumentu XML (XDT)](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html), która jest funkcją, która umożliwia przekształcanie pliku `Web.config` z ustawień deweloperskich na ustawienia produkcyjne. Ustawienia transformacji są określone w plikach transformacji o nazwach `web.debug.config`, `web.release.config`i tak dalej. (Nazwy tych plików są zgodne z konfiguracjami programu MSBuild). Plik transformacji zawiera tylko zmiany, które należy wprowadzić do wdrożonego pliku `Web.config`. Zmiany należy określić przy użyciu prostej składni.

Poniższy przykład przedstawia część pliku `web.release.config`, który może zostać wygenerowany do wdrożenia konfiguracji wydania. Słowo kluczowe Replace w przykładzie określa, że podczas wdrażania węzeł *ConnectionString* w pliku `Web.config` zostanie zastąpiony wartościami wymienionymi w przykładzie.

[!code-xml[Main](overview/samples/sample102.xml)]

Aby uzyskać więcej informacji, zobacz [składnia transformacji Web. config dla wdrożenia projektu aplikacji sieci](https://msdn.microsoft.com/library/dd465326%28VS.100%29.aspx) Web w <a id="0.2_a"></a> witrynie MSDN Web i[Web Deployment: Web. config Transformation](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html) in Vishal Joshi.

<a id="0.2__Toc224729058"></a><a id="0.2__Toc253429295"></a><a id="0.2__Toc243304666"></a>

### <a name="database-deployment"></a>Wdrażanie bazy danych

Pakiet wdrożeniowy programu Visual Studio 2010 może zawierać zależności dotyczące SQL Server baz danych. W ramach definicji pakietu podajesz parametry połączenia dla źródłowej bazy danych. Podczas tworzenia pakietu sieci Web program Visual Studio 2010 tworzy skrypty SQL dla schematu bazy danych i opcjonalnie dla danych, a następnie dodaje je do pakietu. Można również udostępnić niestandardowe skrypty SQL i określić sekwencję, w jakiej powinny być uruchamiane na serwerze. W czasie wdrażania należy podać parametry połączenia, które są odpowiednie dla serwera docelowego; proces wdrażania używa tych parametrów połączenia do uruchamiania skryptów, które tworzą schemat bazy danych i dodają dane.

Ponadto przy użyciu funkcji publikowania jednym kliknięciem można skonfigurować wdrożenie w celu opublikowania bazy danych bezpośrednio po opublikowaniu aplikacji w zdalnej udostępnionej lokacji hostingu. Aby uzyskać więcej informacji, zobacz [jak: Wdrażanie bazy danych z projektem aplikacji sieci](https://msdn.microsoft.com/library/dd465343%28VS.100%29.aspx) Web w witrynie sieci Web MSDN i [wdrażanie bazy danych za pomocą programu VS 2010](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html) na blogu Vishal Joshi.

<a id="0.2__Toc224729059"></a><a id="0.2__Toc253429296"></a><a id="0.2__Toc243304667"></a>

### <a name="one-click-publish-for-web-applications"></a>Publikowanie aplikacji sieci Web jednym kliknięciem

Program Visual Studio 2010 umożliwia również publikowanie aplikacji sieci Web na serwerze zdalnym za pomocą zdalnej usługi zarządzania usługami IIS. Można utworzyć profil publikowania dla konta hostingu lub do testowania serwerów lub serwerów przejściowych. Każdy profil może bezpiecznie zapisywać odpowiednie poświadczenia. Następnie można wykonać wdrożenie na dowolnym serwerze docelowym za pomocą jednego kliknięcia, korzystając z paska narzędzi do publikowania w sieci Web. Program Visual Studio 2010 umożliwia również publikowanie przy użyciu wiersza polecenia programu MSBuild. Pozwala to na skonfigurowanie środowiska kompilacji zespołu w taki sposób, aby obejmowało publikowanie w modelu ciągłej integracji.

Aby uzyskać więcej informacji, zobacz [jak: wdrażanie projektu aplikacji sieci Web za pomocą jednego kliknięcia przycisk Publikuj i Web Deploy](https://msdn.microsoft.com/library/dd465337%28VS.100%29.aspx) w witrynie MSDN w sieci Web i w [sieci Web 1 — kliknij pozycję Publikuj w programie vs 2010](http://vishaljoshi.blogspot.com/2009/05/web-1-click-publish-with-vs-2010.html) na blogu Vishal Joshi. Aby wyświetlić prezentacje wideo dotyczące wdrażania aplikacji sieci Web w programie Visual Studio 2010, zobacz artykuł [VS 2010 for Web Developer](http://vishaljoshi.blogspot.com/2008/12/vs-2010-for-web-developer-previews.html) Preview w blogu Vishal Joshi.

<a id="0.2__Toc224729060"></a><a id="0.2__Toc253429297"></a><a id="0.2__Toc243304668"></a>

### <a name="resources"></a>Zasoby

Poniższe witryny sieci Web zawierają dodatkowe informacje na temat ASP.NET 4 i programu Visual Studio 2010.

- [ASP.NET 4](https://msdn.microsoft.com/library/ee532866%28VS.100%29.aspx) — Oficjalna dokumentacja dla ASP.NET 4 w witrynie MSDN w sieci Web.
- [https://www.asp.net/](https://www.asp.net/) — witryna sieci Web zespołu ASP.NET.
- [https://www.asp.net/dynamicdata/](https://msdn.microsoft.com/library/cc488545.aspx) i [ASP.NET dynamiczną mapę zawartości danych](https://msdn.microsoft.com/library/cc488545%28VS.100%29.aspx) — zasoby Online w witrynie zespołu ASP.NET i w oficjalnej dokumentacji dotyczącej ASP.NET danych dynamicznych.
- [https://www.asp.net/ajax/](../../ajax/index.md) — główny zasób sieci Web do programowania aplikacji ASP.NET AJAX.
- [https://blogs.msdn.com/webdevtools/](https://blogs.msdn.com/webdevtools/) — Blog zespołu programu Visual Web Developer, który zawiera informacje o funkcjach w programie Visual Studio 2010.
- [ASP.NET webstack](https://github.com/aspnet/AspNetWebStack) — główny zasób sieci Web na potrzeby wersji zapoznawczej programu ASP.NET.

<a id="0.2__Toc224729061"></a><a id="0.2__Toc253429298"></a><a id="0.2__Toc243304669"></a>

## <a name="disclaimer"></a>Zastrzeżenie

Ten dokument jest wstępny i może ulec zmianie w odróżnieniu od ostatecznej wersji handlowej oprogramowania opisanego w niniejszej umowie.

Informacje zawarte w tym dokumencie przedstawiają bieżący widok firmy Microsoft Corporation dotyczącej omawianych problemów w dniu publikacji. Ponieważ firma Microsoft musi reagować na zmieniające się warunki rynkowe, nie powinna być interpretowana jako zobowiązanie w ramach firmy Microsoft, a firma Microsoft nie gwarantuje dokładności informacji przedstawionych po dacie publikacji.

Ten oficjalny dokument jest przeznaczony wyłącznie do celów informacyjnych. FIRMA MICROSOFT NIE UDZIELA ŻADNYCH GWARANCJI, WYRAŹNYCH, DOROZUMIANYCH ANI USTAWOWYCH, W ODNIESIENIU DO INFORMACJI ZAWARTYCH W TYM DOKUMENCIE.

Zgodnie ze wszystkimi obowiązującymi prawami autorskimi użytkownik jest odpowiedzialny za użytkownika. Bez ograniczania prawa w ramach praw autorskich, żadna część tego dokumentu nie może zostać odbudowana, zapisana lub wprowadzona w systemie pobierania ani przesłana w jakiejkolwiek formie ani w jakikolwiek sposób (elektroniczny, mechaniczny, z kopiowaniem, nagrań lub w inny sposób) lub w dowolnym celu. bez wyraźnej pisemnej zgody firmy Microsoft Corporation.

Firma Microsoft może mieć patenty, zgłoszenia patentowe, znaki towarowe, prawa autorskie lub inne prawa własności intelektualnej, w tym dokument. O ile nie zostało to wyraźnie określone w pisemnej umowie licencyjnej firmy Microsoft, dostarczenie tego dokumentu nie daje Licencjobiorcy żadnych licencji na te patenty, znaki towarowe, prawa autorskie lub inną własność intelektualną.

O ile nie zaznaczono inaczej, Przykładowe firmy, organizacje, produkty, nazwy domen, adresy e-mail, logo, osoby, miejsca i zdarzenia wymienione w tym dokumencie są fikcyjne i nie są powiązane z rzeczywistymi firmami, organizacjami, produktami, nazwami domen, adresami e-mail adres, logo, osoba, miejsce lub zdarzenie są zamierzone lub wywnioskowane.

© 2009 Microsoft Corporation. Wszelkie prawa zastrzeżone.

Microsoft i Windows są zastrzeżonymi znakami towarowymi lub znakami towarowymi firmy Microsoft Corporation w Stany Zjednoczone i/lub innych krajach.

Nazwy rzeczywistych firm i produktów wymienionych w niniejszym dokumencie mogą być znakami towarowymi odpowiednich właścicieli.

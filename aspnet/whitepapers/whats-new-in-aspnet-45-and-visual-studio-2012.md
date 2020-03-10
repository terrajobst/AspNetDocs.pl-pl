---
uid: whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
title: Co nowego w ASP.NET 4,5 i Visual Studio 2012 | Microsoft Docs
author: rick-anderson
description: W tym dokumencie opisano nowe funkcje i ulepszenia wprowadzane w programie ASP.NET 4,5. Opisano w nim również ulepszenia dotyczące programowania w sieci Web...
ms.author: riande
ms.date: 02/29/2012
ms.assetid: ba1fabb4-31a3-4ebf-8327-41a6bbba6eaf
msc.legacyurl: /whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
msc.type: content
ms.openlocfilehash: 32fbf7c25b00f3f0796c4c3fdd38ca2a86c89199
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78526678"
---
# <a name="whats-new-in-aspnet-45-and-visual-studio-2012"></a>Co nowego w platformie ASP.NET 4.5 i programie Visual Studio 2012

> W tym dokumencie opisano nowe funkcje i ulepszenia wprowadzane w programie ASP.NET 4,5. Opisano w nim również ulepszenia dotyczące programowania w sieci Web w programie Visual Studio 2012. Ten dokument został pierwotnie opublikowany w dniu 29 lutego 2012.

- [Środowisko uruchomieniowe ASP.NET Core i struktura](#_Toc318097372)

    - [Asynchroniczne odczytywanie i zapisywanie żądań i odpowiedzi HTTP](#_Toc318097373)
    - [Ulepszenia obsługi żądania HttpRequest](#_Toc318097374)
    - [Asynchroniczne opróżnianie odpowiedzi](#_Toc318097375)
    - [Obsługa *oczekujących* i modułów asynchronicznych i programów obsługi opartych na *zadaniach*](#_Toc318097376)
    - [Asynchroniczne moduły HTTP](#_Toc318097377)
    - [Asynchroniczne programy obsługi HTTP](#_Toc318097378)
    - [Nowe funkcje walidacji żądania ASP.NET](#_Toc318097379)
    - [Odroczone ("z opóźnieniem") Walidacja żądania](#_Toc318097380)
    - [Obsługa niezweryfikowanych żądań](#_Toc318097381)
    - [Biblioteka AntiXSS](#_Toc318097382)
    - [Obsługa protokołu WebSockets](#_Toc318097383)
    - [Tworzenie pakietów i minifikacja](#_Toc318097384)
    - [Ulepszenia wydajności hostingu w sieci Web](#_Toc_perf)

        - [Kluczowe czynniki wydajności](#_Toc_perf_1)
        - [Wymagania dotyczące nowych funkcji wydajności](#_Toc_perf_2)
        - [Udostępnianie wspólnych zestawów](#_Toc_perf_3)
        - [Używanie wielordzeniowej kompilacji JIT do szybszego uruchamiania](#_Toc_perf_4)
        - [Dostrajanie wyrzucania elementów bezużytecznych w celu zoptymalizowania pamięci](#_Toc_perf_5)
        - [Pobieranie z wyprzedzeniem dla aplikacji sieci Web](#_Toc_perf_6)
- [ASP.NET Web Forms](#_Toc318097385)

    - [Silnie typizowane kontrolki danych](#_Toc318097386)
    - [Wiązanie modelu](#_Toc318097387)

        - [Wybieranie danych](#_Toc318097388)
        - [Dostawcy wartości](#_Toc318097389)
        - [Filtrowanie według wartości z kontrolki](#_Toc318097390)
    - [Wyrażenia wiązania danych kodowane w formacie HTML](#_Toc318097391)
    - [Niezauważalna weryfikacja](#_Toc318097392)
    - [Aktualizacje HTML5](#_Toc318097393)
- [ASP.NET MVC 4](#_Toc318097394)
- [ASP.NET Web Pages 2](#_Toc318097395)
- [Wersja Release Candidate programu Visual Studio 2012](#_Toc318097396)

    - [Udostępnianie projektu między programem Visual Studio 2010 i Visual Studio 2012 Release Candidate (zgodność projektu)](#project-compatibility)
    - [Zmiany konfiguracji w szablonach witryny sieci Web ASP.NET 4,5](#Configuration_Changes_In_ASPNET45_Website_Templates)
    - [Natywna obsługa w usługach IIS 7 dla routingu ASP.NET](#Native_Support_In_IIS7_For_ASPNET_Routine)
    - [Edytor HTML](#_Toc318097397)

        - [Zadania inteligentne](#_Toc318097398)
        - [Obsługa poczeka — ARIA](#_Toc318097399)
        - [Nowe fragmenty kodu HTML5](#_Toc318097400)
        - [Wyodrębnij do kontrolki użytkownika](#_Toc318097401)
        - [Funkcja IntelliSense dla Nuggets kodu w atrybutach](#_Toc318097402)
        - [Automatyczna zmiana nazwy pasującego tagu w przypadku zmiany nazwy otwierającego lub zamykającego tagu](#_Toc318097403)
        - [Generowanie programu obsługi zdarzeń](#_Toc318097404)
        - [Inteligentne wcięcie](#_Toc318097405)
        - [Autouzupełnianie uzupełniania instrukcji](#_Toc318097406)
    - [Edytor JavaScript](#_Toc318097407)

        - [Konspekt kodu](#_Toc318097408)
        - [Dopasowywanie nawiasów klamrowych](#_Toc318097409)
        - [Przejdź do definicji](#_Toc318097410)
        - [Obsługa ECMAScript5](#_Toc318097411)
        - [Funkcja IntelliSense modelu DOM](#_Toc318097412)
        - [Przeciążenia sygnatur VSDOC](#_Toc318097413)
        - [Niejawne odwołania](#_Toc318097414)
    - [Edytor CSS](#_Toc318097415)

        - [Autouzupełnianie uzupełniania instrukcji](#_Toc318097416)
        - [Wcięcie hierarchiczne.](#_Toc318097417)
        - [Obsługa CSS Hacks](#_Toc318097418)
        - [Schematy specyficzne dla dostawcy (-moz-,-WebKit)](#_Toc318097419)
        - [Obsługa komentowania i usuwania komentarzy](#_Toc318097420)
        - [Selektor kolorów](#_Toc318097421)
        - [Wstawki kodu](#_Toc318097422)
        - [Regiony niestandardowe](#_Toc318097423)
    - [Inspektor strony](#_Toc318097424)
    - [Publikowanie](#_Toc318097425)

        - [Profile publikowania](#_Toc318097426)
        - [ASP.NET prekompilowania i scalania](#_Toc318097427)
- [IIS Express](#_Toc318097428)
- [Zastrzeżenie](#_Toc318097429)

<a id="_Toc318097372"></a>
## <a name="aspnet-core-runtime-and-framework"></a>Środowisko uruchomieniowe ASP.NET Core i struktura

<a id="_Toc318097373"></a>
### <a name="asynchronously-reading-and-writing-http-requests-and-responses"></a>Asynchroniczne odczytywanie i zapisywanie żądań i odpowiedzi HTTP

ASP.NET 4 wprowadził możliwość odczytu jednostki żądania HTTP jako strumienia przy użyciu metody *HttpRequest. GetBufferlessInputStream* . Ta metoda zapewniała dostęp strumieniowy do jednostki żądania. Jednak wykonywane synchronicznie, który wiąże wątek na czas trwania żądania.

ASP.NET 4,5 obsługuje możliwość odczytywania strumieni asynchronicznie w jednostce żądania HTTP i możliwość asynchronicznego opróżniania. ASP.NET 4,5 daje również możliwość podwójnego buforowania jednostki żądania HTTP, która zapewnia łatwiejszą integrację z podrzędnymi obsługami HTTP, takimi jak programy obsługi stron aspx i kontrolery ASP.NET MVC.

<a id="_Toc318097374"></a>
#### <a name="improvements-to-httprequest-handling"></a>Ulepszenia obsługi żądania HttpRequest

Odwołanie strumieniowe zwrócone przez ASP.NET 4,5 z elementu *HttpRequest. GetBufferlessInputStream* obsługuje metody odczytu synchronicznego i asynchronicznego. Obiekt *Stream* zwrócony z *GetBufferlessInputStream* teraz implementuje metody BeginRead i EndRead. Asynchroniczne metody *strumieni* umożliwiają asynchroniczne odczytywanie jednostki żądania w fragmentach, podczas gdy ASP.NET uwalnia bieżący wątek między każdą iteracją asynchronicznej pętli odczytu.

ASP.NET 4,5 również dodał metodę towarzyszącą do odczytywania jednostki żądania w sposób buforowany: *HttpRequest. GetBufferedInputStream*. To nowe Przeciążenie działa jak *GetBufferlessInputStream*, obsługujące odczyty synchroniczne i asynchroniczne. Jednak w miarę odczytywania *GetBufferedInputStream* kopiuje także bajty jednostek do ASP.NET wewnętrznych buforów, tak aby moduły podrzędne i programy obsługi nadal mogły uzyskać dostęp do jednostki żądania. Na przykład jeśli jakiś kod nadrzędny w potoku już odczytał jednostkę żądania przy użyciu *GetBufferedInputStream*, można nadal używać elementu *HttpRequest. form* lub *HttpRequest. Files*. Dzięki temu można wykonać asynchroniczne przetwarzanie na żądanie (na przykład przesłać strumieniowo duży plik do bazy danych), ale nadal uruchamiaj strony aspx i kontrolery MVC ASP.NET.

<a id="_Toc318097375"></a>
#### <a name="asynchronously-flushing-a-response"></a>Asynchroniczne opróżnianie odpowiedzi

Wysyłanie odpowiedzi do klienta HTTP może zająć dużo czasu, gdy klient jest daleko lub ma połączenie o niskiej przepustowości. Zwykle ASP.NET buforuje bajty odpowiedzi, ponieważ są one tworzone przez aplikację. ASP.NET następnie wykonuje pojedyncze operacje wysyłania w oddzielnym buforze na bardzo zakończenie przetwarzania żądania.

Jeśli odpowiedź buforowana jest duża (na przykład przesyła strumieniowo duży plik do klienta), należy okresowo wywołać *HttpResponse. Flush* , aby wysłać buforowane dane wyjściowe do klienta i zachować użycie pamięci w obszarze kontroli. Jednak ponieważ *opróżnianie* jest wywołaniem synchronicznym, iteracyjnie wywołujące metodę *Flush* nadal zużywa wątek na czas trwania potencjalnie długotrwałych żądań.

ASP.NET 4,5 dodaje obsługę operacji opróżniania asynchronicznie przy użyciu metod *BeginFlush* i *EndFlush* klasy *HttpResponse* . Korzystając z tych metod, można tworzyć moduły asynchroniczne i programy obsługi asynchroniczne, które przyrostowo przesyłają dane do klienta bez konieczności korzystania z wątków systemu operacyjnego. W przypadku wywołań *BeginFlush* i *EndFlush* , ASP.NET zwalnia bieżący wątek. Znacznie zmniejsza to łączną liczbę aktywnych wątków, które są konieczne w celu obsługi długotrwałych pobrań HTTP.

<a id="_Toc318097376"></a>
### <a name="support-for-await-and-task---based-asynchronous-modules-and-handlers"></a>Obsługa *oczekujących* i modułów asynchronicznych i programów obsługi opartych na *zadaniach*

W .NET Framework 4 wprowadzono asynchroniczną koncepcję programowania nazywaną *zadaniem*. Zadania są reprezentowane przez typ *zadania* i powiązane typy w przestrzeni nazw *System. Threading. Tasks* . .NET Framework 4,5 kompiluje na tym komputerze z ulepszeniami kompilatora, które ułatwiają pracę z obiektami *zadań* . W .NET Framework 4,5 kompilatory obsługują dwa nowe słowa kluczowe: *await* i *Async*. Słowo kluczowe *await* jest skrótową składnią wskazującą, że fragment kodu powinien asynchronicznie oczekiwać na inne fragmenty kodu. Słowo kluczowe *Async* reprezentuje wskazówkę, której można użyć do oznaczenia metod jako metod asynchronicznych opartych na zadaniach.

Połączenie obiektów *await*, *Async*i *Task* ułatwia pisanie kodu asynchronicznego w programie .NET 4,5. ASP.NET 4,5 obsługuje te uproszczenia przy użyciu nowych interfejsów API, które umożliwiają pisanie asynchronicznych modułów HTTP i asynchronicznych programów obsługi protokołu HTTP przy użyciu nowych ulepszeń kompilatora.

<a id="_Toc318097377"></a>
#### <a name="asynchronous-http-modules"></a>Asynchroniczne moduły HTTP

Załóżmy, że chcesz wykonać asynchroniczne działanie w metodzie, która zwraca obiekt *zadania* . Poniższy przykład kodu definiuje metodę asynchroniczną, która wywołuje asynchroniczne wywołanie pobierania strony głównej firmy Microsoft. Zwróć uwagę na użycie słowa kluczowego *Async* w sygnaturze metody i wywołaniu *await* do *DownloadStringTaskAsync*.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample1.cs)]

To wszystko, co trzeba napisać — .NET Framework automatycznie obsłużyć odwinięcia stosu wywołań podczas oczekiwania na ukończenie pobierania, a także automatycznie przywraca stos wywołań po zakończeniu pobierania.

Teraz Załóżmy, że chcesz użyć tej metody asynchronicznej w asynchronicznym module HTTP ASP.NET. ASP.NET 4,5 zawiera metodę pomocnika (*EventHandlerTaskAsyncHelper*) i nowy typ delegata (*TaskEventHandler*), który służy do integrowania metod asynchronicznych opartych na zadaniach ze starszym, asynchronicznym modelem programowania udostępnionym przez potoku ASP.net http. Ten przykład pokazuje, jak:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample2.cs)]

<a id="_Toc318097378"></a>
#### <a name="asynchronous-http-handlers"></a>Asynchroniczne programy obsługi HTTP

Tradycyjnym podejściem do pisania procedur obsługi asynchronicznej w ASP.NET jest zaimplementowanie interfejsu *IHttpAsyncHandler* . ASP.NET 4,5 wprowadza asynchroniczny typ podstawowy *HttpTaskAsyncHandler* , z którego można pochodzić, co znacznie ułatwia pisanie programów obsługi asynchronicznej.

Typ *HttpTaskAsyncHandler* jest abstrakcyjny i wymaga przesłonięcia metody *ProcessRequestAsync* . ASP.NET wewnętrznie ma na celu integrację sygnatury zwrotnej (obiektu *Task* ) *ProcessRequestAsync* z starszym asynchronicznym modelem programowania używanym przez potok ASP.NET.

Poniższy przykład pokazuje, jak można użyć *zadania* i *oczekiwania* jako części implementacji asynchronicznej procedury obsługi http:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample3.cs)]

<a id="_Toc318097379"></a>
### <a name="new-aspnet-request-validation-features"></a>Nowe funkcje walidacji żądania ASP.NET

Domyślnie ASP.NET wykonuje walidację żądania — sprawdza żądania, aby wyszukać znaczniki lub skrypty w polach, nagłówkach, plikach cookie i tak dalej. W przypadku wykrycia elementu ASP.NET zgłasza wyjątek. Działa to jako pierwszy wiersz obrony przed potencjalnymi atakami na skrypty między lokacjami.

ASP.NET 4,5 ułatwia wybiórcze odczytywanie niezweryfikowanych danych żądań. ASP.NET 4,5 integruje także popularną bibliotekę AntiXSS, która była wcześniej biblioteką zewnętrzną.

Deweloperzy często pytają o możliwość selektywnego wyłączania weryfikacji żądań dla swoich aplikacji. Jeśli na przykład aplikacja jest oprogramowaniem forum, możesz chcieć zezwolić użytkownikom na przesyłanie wpisów i komentarzy na forum w formacie HTML, ale nadal upewnij się, że Walidacja żądań sprawdza wszystko inne.

ASP.NET 4,5 wprowadza dwie funkcje, które ułatwiają wybiórczą współpracę z niezweryfikowanymi danymi wejściowymi: odroczone ("z opóźnieniem") Walidacja żądań i dostęp do niezweryfikowanych danych żądania.

<a id="_Toc318097380"></a>
#### <a name="deferred-lazy-request-validation"></a>Odroczone ("z opóźnieniem") Walidacja żądania

W ASP.NET 4,5, domyślnie wszystkie dane żądania podlegają weryfikacji żądania. Można jednak skonfigurować aplikację, aby odroczyć sprawdzanie poprawności żądań do momentu rzeczywistego dostępu do danych żądania. (Jest to czasami nazywane sprawdzaniem żądań z opóźnieniem na podstawie warunków, takich jak ładowanie z opóźnieniem dla niektórych scenariuszy danych). Można skonfigurować aplikację tak, aby korzystała z odroczonej walidacji w pliku Web. config, ustawiając atrybut *requestValidationMode* na 4,5 w elemencie *httpRUntime* , jak w poniższym przykładzie:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample4.xml)]

Gdy tryb walidacji żądania jest ustawiony na 4,5, walidacja żądania jest wyzwalana tylko dla określonej wartości żądania i tylko wtedy, gdy kod uzyskuje dostęp do tej wartości. Na przykład, jeśli kod pobiera wartość Request. form ["forum\_post"], walidacja żądania jest wywoływana tylko dla tego elementu w kolekcji formularzy. Żaden z pozostałych elementów kolekcji *formularzy* nie zostanie sprawdzony. We wcześniejszych wersjach programu ASP.NET sprawdzanie poprawności żądania zostało wyzwolone dla całej kolekcji żądań, gdy uzyskano dostęp do dowolnego elementu w kolekcji. Nowe zachowanie ułatwia różnym składnikom aplikacji przeglądanie różnych fragmentów danych żądania bez wyzwalania weryfikacji żądań na innych fragmentach.

<a id="_Toc318097381"></a>
#### <a name="support-for-unvalidated-requests"></a>Obsługa niezweryfikowanych żądań

Weryfikacja odroczonego żądania nie rozwiązuje problemu selektywnego pomijania sprawdzania poprawności żądania. Wywołanie metody Request. form ["forum\_post"] nadal wyzwala weryfikację żądań dla tej konkretnej wartości żądania. Jednak możesz chcieć uzyskać dostęp do tego pola bez wyzwalania walidacji, ponieważ chcesz zezwolić na adiustację w tym polu.

Aby to umożliwić, ASP.NET 4,5 obsługuje teraz niezweryfikowany dostęp do danych żądania. ASP.NET 4,5 zawiera nową właściwość kolekcji *niezweryfikowanej* w klasie *HttpRequest* . Ta kolekcja zapewnia dostęp do wszystkich wspólnych wartości danych żądania, takich jak *form*, *QueryString*, *cookies*i *URL*.

Za pomocą przykładowego forum, aby można było odczytać niezweryfikowane dane żądania, najpierw musisz skonfigurować aplikację tak, aby korzystała z nowego trybu walidacji żądania:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample5.xml)]

Następnie można użyć właściwości *HttpRequest. unvalidated* , aby odczytać niezweryfikowaną wartość formularza:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample6.cs)]

> [!WARNING]
> Zabezpieczenia — *korzystaj z niezweryfikowanych danych żądań z opieką!* ASP.NET 4,5 dodał niezweryfikowane właściwości i kolekcje żądania, aby ułatwić uzyskiwanie dostępu do bardzo konkretnych niezweryfikowanych danych żądań. Jednak nadal należy przeprowadzić niestandardowe sprawdzanie poprawności danych żądań nieprzetworzonych, aby upewnić się, że niebezpieczny tekst nie jest renderowany dla użytkowników.

<a id="_Toc318097382"></a>
### <a name="antixss-library"></a>Biblioteka AntiXSS

Ze względu na popularność biblioteki AntiXSS firmy Microsoft, ASP.NET 4,5 teraz zawiera podstawowe procedury kodowania z wersji 4,0 tej biblioteki.

Procedury kodowania są implementowane przez typ *AntiXssEncoder* w nowej przestrzeni nazw *System. Web. Security. AntiXSS* . Możesz użyć typu *AntiXssEncoder* bezpośrednio, wywołując dowolną metodę kodowania statycznego, która jest zaimplementowana w typie. Jednak najprostszym podejściem do korzystania z nowych procedur antyxss jest skonfigurowanie aplikacji ASP.NET do używania klasy *AntiXssEncoder* domyślnie. Aby to zrobić, Dodaj następujący atrybut do pliku Web. config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample7.xml)]

Gdy atrybut *EncoderType* jest ustawiony do używania typu *AntiXssEncoder* , wszystkie Kodowanie wyjściowe w ASP.NET automatycznie używa nowych procedur kodowania.

Są to części zewnętrznej biblioteki AntiXSS, które zostały włączone do ASP.NET 4,5:

- *HtmlEncode*, *HtmlFormUrlEncode*i *HtmlAttributeEncode*
- *XmlAttributeEncode* i *XmlEncode*
- *UrlEncode* i *UrlPathEncode* (nowy)
- *CssEncode*

<a id="_Toc318097383"></a>
### <a name="support-for-websockets-protocol"></a>Obsługa protokołu WebSockets

Protokół WebSockets to oparty na standardach protokół sieciowy, który definiuje sposób nawiązywania bezpiecznej komunikacji dwukierunkowej między klientem a serwerem za pośrednictwem protokołu HTTP. Firma Microsoft pracowała zarówno z organizacjami standardowymi IETF, jak i W3C, aby pomóc w definiowaniu protokołu. Protokół WebSockets jest obsługiwany przez dowolnego klienta (nie tylko przeglądarki), dzięki czemu firma Microsoft ponosi znaczne zasoby obsługujące protokół WebSockets dla klienta i systemów operacyjnych urządzeń przenośnych.

Protokół WebSockets ułatwia tworzenie długotrwałych transferów danych między klientem a serwerem. Na przykład pisanie aplikacji czatu jest znacznie łatwiejsze, ponieważ można ustanowić prawdziwie długotrwałe połączenie między klientem a serwerem. Nie trzeba korzystać z obejść, takich jak okresowe sondowanie czy wykonywanie długotrwałych połączeń HTTP w celu symulowania zachowania gniazda.

ASP.NET 4,5 i IIS 8 obejmują obsługę elementów WebSockets niskiego poziomu, co umożliwia deweloperom ASP.NET używanie zarządzanych interfejsów API do asynchronicznego odczytywania i zapisywania danych typu String i binary w obiekcie WebSockets. W przypadku ASP.NET 4,5 istnieje nowa przestrzeń nazw *System. Web. WebSockets* , która zawiera typy umożliwiające pracę z protokołem usługi WebSockets.

Klient przeglądarki nawiązuje połączenie z usługą WebSockets, tworząc obiekt obiektu *WebSocket* modelu dom, który wskazuje na adres URL w aplikacji ASP.NET, jak w poniższym przykładzie:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample8.cs)]

Punkty końcowe elementów WebSockets można tworzyć w ASP.NET przy użyciu dowolnego rodzaju modułu lub procedury obsługi. W poprzednim przykładzie użyto pliku. ashx, ponieważ pliki ASHX są szybkim sposobem tworzenia programu obsługi.

Zgodnie z protokołem WebSockets aplikacja ASP.NET akceptuje żądanie elementu WebSockets klienta, wskazując, że żądanie powinno zostać uaktualnione z żądania HTTP GET do żądania WebSockets. Oto przykład:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample9.cs)]

Metoda *AcceptWebSocketRequest* akceptuje delegata funkcji, ponieważ ASP.NET powoduje odwinięcia bieżącego żądania HTTP, a następnie przekazanie kontroli do delegata funkcji. Koncepcyjnie takie podejście jest podobne do sposobu korzystania z *System. Threading. Thread*, gdzie definiujesz delegata uruchomienia wątku, w którym jest wykonywana działa w tle.

Gdy ASP.NET i klient pomyślnie ukończy uzgadnianie gniazd WebSockets, ASP.NET wywołuje delegata, a aplikacja WebSockets zacznie działać. Poniższy przykład kodu pokazuje prostą aplikację echo, która używa wbudowanych funkcji obsługi sieci WebSockets w ASP.NET:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample10.cs)]

Obsługa w programie .NET 4,5 dla słowa kluczowego *await* i asynchronicznych operacji opartych na zadaniach jest naturalna dla pisania aplikacji sieci WebSockets. Przykład kodu pokazuje, że żądanie elementu WebSockets działa całkowicie asynchronicznie wewnątrz ASP.NET. Aplikacja czeka asynchronicznie na wysłanie komunikatu z klienta, wywołując *gniazdo await. ReceiveAsync*. Podobnie można wysłać komunikat asynchroniczny do klienta, wywołując *gniazdo await. SendAsync*.

W przeglądarce aplikacja otrzymuje komunikaty dotyczące obiektów WebSockets za pomocą funkcji *OnMessage* . Aby wysłać komunikat z przeglądarki, należy wywołać metodę *send* typu dom *protokołu WebSocket* , jak pokazano w poniższym przykładzie:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample11.cs)]

W przyszłości możemy wydać aktualizacje tej funkcji, które dzielą się niektórym kodowaniem niskiego poziomu, które jest wymagane w tej wersji dla aplikacji usługi WebSockets.

<a id="_Toc318097384"></a>
### <a name="bundling-and-minification"></a>Tworzenie pakietów i minimalizowanie

Zgrupowanie umożliwia łączenie poszczególnych plików JavaScript i CSS w pakiet, który może być traktowany jak pojedynczy plik. Minifikacja skrapla pliki JavaScript i CSS, usuwając odstępy i inne znaki, które nie są wymagane. Te funkcje działają w przypadku formularzy sieci Web, ASP.NET MVC i stron sieci Web.

Zbiory są tworzone przy użyciu klasy pakietu lub jednej z jej klas podrzędnych, ScriptBundle i StyleBundle. Po skonfigurowaniu wystąpienia pakietu pakiet jest udostępniany do przychodzących żądań przez dodanie go do globalnego wystąpienia programubinding. W domyślnych szablonach konfiguracja pakietu jest wykonywana w pliku BundleConfig. Ta konfiguracja domyślna tworzy pakiety dla wszystkich skryptów podstawowych i plików CSS używanych przez szablony.

Pakiety są przywoływane w widokach przy użyciu jednej z kilku możliwych metod pomocnika. Aby można było obsłużyć renderowanie innego znacznika dla pakietu w trybie Debug i Release, klasy ScriptBundle i StyleBundle mają metodę pomocnika, renderowanie. W trybie debugowania renderowanie będzie generować znaczniki dla każdego zasobu w zbiorze. W trybie wydania Render wygeneruje pojedynczy element znaczników dla całego pakietu. Przełączanie między trybem debugowania i zwalniania można osiągnąć, modyfikując atrybut Debug elementu compilation w pliku Web. config, jak pokazano poniżej:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample12.xml)]

Ponadto Włączanie lub wyłączanie optymalizacji można ustawić bezpośrednio za pośrednictwem właściwości pakiet. EnableOptimizations.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample13.cs)]

Gdy pliki są powiązane, są najpierw sortowane alfabetycznie (sposób ich wyświetlania w **Eksplorator rozwiązań**). Są one następnie zorganizowane w taki sposób, że znane biblioteki i ich rozszerzenia niestandardowe (takie jak jQuery, MooTools i Dojo) są ładowane jako pierwsze. Na przykład ostateczna kolejność grupowania folderów skryptów, jak pokazano powyżej, będzie następująca:

1. jquery-1.6.2.js
2. jquery-ui.js
3. jquery.tools.js
4. a. js

Pliki CSS są również sortowane alfabetycznie, a następnie ponownie zorganizowane, dzięki czemu Zresetuj. CSS i normalizing. CSS jest wcześniejszy niż każdy inny plik. Ostatecznym sortowaniem grupowania folderu style pokazanego powyżej będzie:

1. Zresetuj. CSS
2. Content. CSS
3. Forms. CSS
4. globals.css
5. menu.css
6. Style. CSS

<a id="_Toc_perf"></a>
### <a name="performance-improvements-for-web-hosting"></a>Ulepszenia wydajności hostingu w sieci Web

.NET Framework 4,5 i Windows 8 wprowadzają funkcje, które mogą pomóc w osiągnięciu znacznego zwiększenia wydajności obciążeń serwera sieci Web. Obejmuje to zmniejszenie (do 35%) zarówno w czasie uruchamiania, jak i w pamięci w witrynach hostingu w sieci Web, które używają ASP.NET.

<a id="_Toc_perf_1"></a>
#### <a name="key-performance-factors"></a>Kluczowe czynniki wydajności

W idealnym przypadku wszystkie witryny sieci Web powinny być aktywne i w pamięci, aby zapewnić szybką odpowiedź na kolejne żądanie. Czynniki, które mogą mieć wpływ na czas odpowiedzi witryny, to m.in.:

- Czas ponownego uruchomienia lokacji po odtworzeniu puli aplikacji. Jest to czas potrzebny do uruchomienia procesu serwera sieci Web dla lokacji, gdy zbiory lokacji nie znajdują się już w pamięci. (Zestawy platform nadal znajdują się w pamięci, ponieważ są używane przez inne Lokacje). Ta sytuacja jest określana jako "zimna lokacja, uruchamianie platformy ciepłej" lub po prostu "Uruchamianie zimnej witryny".
- Ilość pamięci zajmowanej przez lokację. Są to "użycie pamięci na lokację" lub "nieudostępniany zestaw roboczy".

Nowe ulepszenia wydajności koncentrują się na obu tych czynnikach.

<a id="_Toc_perf_2"></a>
#### <a name="requirements-for-new-performance-features"></a>Wymagania dotyczące nowych funkcji wydajności

Wymagania dotyczące nowych funkcji można podzielić na następujące kategorie:

- Ulepszenia działające na .NET Framework 4.
- Ulepszenia wymagające .NET Framework 4,5, ale mogą być uruchamiane w dowolnej wersji systemu Windows.
- Ulepszenia, które są dostępne tylko w .NET Framework 4,5 działające w systemie Windows 8.

Wydajność zwiększa się wraz z każdym poziomem ulepszeń, które można włączyć.

Niektóre ulepszenia w .NET Framework 4,5 wykorzystują szersze funkcje wydajności, które są stosowane również do innych scenariuszy.

<a id="_Toc_perf_3"></a>
#### <a name="sharing-common-assemblies"></a>Udostępnianie wspólnych zestawów

**Wymaganie**: .NET Framework 4 i Visual Studio 11 Developer Preview SDK

Różne lokacje na serwerze często używają tych samych zestawów pomocników (na przykład zestawów z zestawu startowego lub przykładowej aplikacji). Każda lokacja ma własną kopię tych zestawów w katalogu bin. Chociaż kod obiektu dla zestawów jest identyczny, są one fizycznie oddzielnymi zestawami, więc każdy zestaw musi być odczytany oddzielnie podczas uruchamiania lokacji zimnej i oddzielnie w pamięci.

Nowa funkcja InterNIC rozwiązuje ten problem i zmniejsza zarówno wymagania dotyczące pamięci RAM, jak i czas ładowania. Informowanie umożliwia systemowi Windows zachowanie pojedynczej kopii każdego zestawu w systemie plików, a poszczególne zestawy w folderach bin lokacji są zastępowane symbolicznymi linkami do pojedynczej kopii. Jeśli indywidualna lokacja wymaga odrębnej wersji zestawu, łącze symboliczne zostanie zastąpione nową wersją zestawu i dotyczy tylko tej lokacji.

Udostępnianie zestawów przy użyciu linków symbolicznych wymaga nowego narzędzia o nazwie ASPNET\_InterNIC. exe, które umożliwia tworzenie i zarządzanie magazynem zespołów z zespołami. Jest on dostarczany jako część zestawu SDK programu Visual Studio 11 Developer Preview. (Jednak będzie działał w systemie, w którym zainstalowano tylko .NET Framework 4, przy założeniu, że zainstalowano najnowszą [aktualizację](https://support.microsoft.com/kb/2468871)).

Aby upewnić się, że wszystkie kwalifikujące się zestawy zostały poddane oddziału, należy regularnie uruchamiać ASPNET\_stażyst. exe (na przykład raz w tygodniu jako zaplanowane zadanie). Typowym zastosowaniem jest następujące:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample14.cmd)]

Aby wyświetlić wszystkie opcje, uruchom narzędzie bez argumentów.

<a id="_Toc_perf_4"></a>
#### <a name="using-multi-core-jit-compilation-for-faster-startup"></a>Używanie wielordzeniowej kompilacji JIT do szybszego uruchamiania

**Wymaganie**: .NET Framework 4,5

W przypadku uruchamiania zimnej lokacji nie tylko zestawy muszą być odczytane z dysku, ale lokacja musi być skompilowana w trybie JIT. W przypadku złożonej witryny może to zwiększyć znaczne opóźnienia. Nową techniką ogólnego przeznaczenia w .NET Framework 4,5 zmniejszają te opóźnienia poprzez rozproszenie kompilacji JIT na dostępne rdzenie procesora. Jest to tak dużo i tak wcześnie, jak to możliwe, przy użyciu informacji zebranych podczas poprzednich uruchomień lokacji. Ta funkcja implementowana przez metodę [System. Runtime. ProfileOptimization. StartProfile](https://msdn.microsoft.com/library/system.runtime.profileoptimization.startprofile(VS.110).aspx) .

Kompilowanie JIT przy użyciu wielu rdzeni jest domyślnie włączone w ASP.NET, więc nie trzeba wykonywać żadnych czynności w celu skorzystania z tej funkcji. Jeśli chcesz wyłączyć tę funkcję, wprowadź następujące ustawienie w pliku Web. config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample15.xml)]

<a id="_Toc_perf_5"></a>
#### <a name="tuning-garbage-collection-to-optimize-for-memory"></a>Dostrajanie wyrzucania elementów bezużytecznych w celu zoptymalizowania pamięci

**Wymaganie**: .NET Framework 4,5

Gdy lokacja jest uruchomiona, jej użycie sterty modułu wyrzucania elementów bezużytecznych (GC) może być istotnym czynnikiem zużycia pamięci. Podobnie jak w przypadku dowolnego modułu wyrzucania elementów bezużytecznych, .NET Framework WYKAZUje kompromisy między czasem procesora (częstotliwość i znaczenie kolekcji) i zużycie pamięci (dodatkowe miejsce używane do nowych, zwolnionych lub bezpłatnych obiektów). W przypadku poprzednich wersji zostały podane wskazówki dotyczące sposobu konfigurowania procedury GC w celu osiągnięcia odpowiedniego salda (na przykład zobacz [ASP.NET 2.0/3.5 udostępnionej konfiguracji hostingu](https://www.iis.net/learn/web-hosting/web-server-for-shared-hosting/aspnet-20-35-shared-hosting-configuration)).

W przypadku .NET Framework 4,5 zamiast wielu ustawień autonomicznych dostępne jest ustawienie konfiguracji zdefiniowane przez obciążenie, które włącza wszystkie poprzednio zalecane ustawienia GC oraz nowe Dostrajanie, które zapewnia dodatkową wydajność dla poszczególnych lokacji. zestaw roboczy.

Aby włączyć dostrajanie pamięci GC, Dodaj następujące ustawienie do pliku Windows\Microsoft.NET\Framework\v4.0.30319\aspnet.config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample16.xml)]

(Jeśli znasz poprzednie wskazówki dotyczące zmian w pliku aspnet. config, zwróć uwagę, że to ustawienie zastępuje stare ustawienia — na przykład nie ma potrzeby ustawiania gcServer, gcConcurrent itp. Nie musisz usuwać starych ustawień.

<a id="_Toc_perf_6"></a>
#### <a name="prefetching-for-web-applications"></a>Pobieranie z wyprzedzeniem dla aplikacji sieci Web

**Wymaganie**: .NET Framework 4,5 działające w systemie Windows 8

W przypadku kilku wydań system Windows dołączył technologię znaną jako [Prefetcher](http://en.wikipedia.org/wiki/Prefetcher) , która zmniejsza koszty uruchamiania aplikacji na dysku. Ponieważ zimne uruchomienie jest problemem głównie dla aplikacji klienckich, ta technologia nie została uwzględniona w systemie Windows Server, która obejmuje tylko składniki, które są niezbędne dla serwera programu. Wstępne pobieranie jest teraz dostępne w najnowszej wersji systemu Windows Server, co umożliwia optymalizację uruchamiania poszczególnych witryn sieci Web.

W przypadku systemu Windows Server Prefetcher nie jest domyślnie włączona. Aby włączyć i skonfigurować Prefetcher dla hostingu w sieci Web o wysokiej gęstości, uruchom następujący zestaw poleceń w wierszu polecenia:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample17.cmd)]

Następnie, aby zintegrować program Prefetcher z aplikacjami ASP.NET, Dodaj następujący plik do pliku Web. config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample18.xml)]

<a id="_Toc318097385"></a>
## <a name="aspnet-web-forms"></a>Formularze sieci Web ASP.NET

<a id="_Toc318097386"></a>
### <a name="strongly-typed-data-controls"></a>Silnie typizowane kontrolki danych

W programie ASP.NET 4,5 formularze sieci Web zawierają kilka ulepszeń dotyczących pracy z danymi. Pierwsze ulepszenie jest jednoznacznie określonym typem kontroli danych. W przypadku formantów formularzy sieci Web w poprzednich wersjach ASP.NET jest wyświetlana wartość związana z danymi przy użyciu funkcji *eval* i wyrażenia powiązania danych:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample19.aspx)]

W przypadku powiązań danych dwukierunkowych należy użyć *powiązania*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample20.aspx)]

W czasie wykonywania te wywołania używają odbicia do odczytu wartości określonego elementu członkowskiego, a następnie wyświetlają wynik w znaczniku. Takie podejście ułatwia powiązanie danych z dowolnymi, niekształtnymi danymi.

Jednak wyrażenia wiązania danych, takie jak ta, nie obsługują takich funkcji, jak IntelliSense dla nazw składowych, nawigowanie (jak przejdź do definicji) lub sprawdzanie czasu kompilacji dla tych nazw.

Aby rozwiązać ten problem, ASP.NET 4,5 dodaje możliwość deklarowania typu danych danych, z którymi jest powiązany formant. W tym celu należy użyć nowej właściwości *ItemType* . Po ustawieniu tej właściwości w zakresie wyrażeń powiązań danych są dostępne dwie nowe zmienne z określonym typem: *Item* i *binditem*. Ze względu na to, że zmienne mają silną wartość, uzyskasz pełne korzyści związane z programowaniem Visual Studio.

Dla dwukierunkowych wyrażeń powiązań danych Użyj zmiennej *binditem* :

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample21.aspx)]

Większość formantów w strukturze formularzy sieci Web ASP.NET, która obsługuje powiązanie danych, została zaktualizowana w celu obsługi właściwości *ItemType* .

<a id="_Toc318097387"></a>
### <a name="model-binding"></a>Powiązanie modelu

Powiązanie modelu rozszerza powiązanie danych w kontrolkach formularzy sieci Web ASP.NET do pracy z dostępem do danych ukierunkowanych na kod. Obejmuje to koncepcje z formantu *ObjectDataSource* i powiązania modelu w ASP.NET MVC.

<a id="_Toc318097388"></a>
#### <a name="selecting-data"></a>Wybieranie danych

Aby skonfigurować kontrolkę danych do używania powiązania modelu w celu wybrania danych, należy ustawić właściwość *SelectMethod* kontrolki na nazwę metody w kodzie strony. Kontrola danych wywołuje metodę w odpowiednim czasie w cyklu życia strony i automatycznie wiąże zwrócone dane. Nie trzeba jawnie wywoływać metody *DataBind* .

W poniższym przykładzie formant *GridView* jest skonfigurowany do korzystania z metody o nazwie *GetCategories*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample22.aspx)]

Tworzysz metodę *GetCategories* w kodzie strony. W przypadku prostej operacji zaznaczania Metoda nie wymaga żadnych parametrów i powinna zwracać obiekt *IEnumerable* lub *IQueryable* . Jeśli nowa właściwość *ItemType* jest ustawiona (która włącza wyrażenia powiązań danych o jednoznacznie określonym typie, jak wyjaśniono w obszarze [kontroli danych silnie wpisanej](#_Toc318097386) ), należy zwrócić ogólne wersje tych interfejsów — *IEnumerable&lt;T&gt;* lub *IQueryable&lt;t&gt;* , z parametrem *t* pasującym do typu właściwości *ItemType* (na przykład: *IQueryable&lt;kategorii&gt;* ).

Poniższy przykład pokazuje kod metody *GetCategories* . W tym przykładzie zastosowano model Code First Entity Framework z przykładową bazą danych Northwind. Kod sprawdza, czy zapytanie zwraca szczegóły pokrewnych produktów dla każdej kategorii za pomocą metody *include* . (Daje to gwarancję, że element *TemplateField* w znaczniku wyświetla liczbę produktów w każdej kategorii, bez konieczności [zaznaczania n + 1](http://stackoverflow.com/questions/97197/what-is-the-n1-selects-problem)).

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample23.cs)]

Po uruchomieniu strony kontrolka *GridView* automatycznie wywołuje metodę *GetCategories* i renderuje zwrócone dane przy użyciu skonfigurowanych pól:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.png)

Ponieważ metoda SELECT zwraca obiekt *IQueryable* , formant *GridView* może dodatkowo manipulować kwerendą przed jej wykonaniem. Na przykład formant *GridView* może dodawać wyrażenia zapytania do sortowania i stronicowania do zwróconego obiektu *IQueryable* przed wykonaniem, aby te operacje były wykonywane przez bazowego dostawcę LINQ. W takim przypadku Entity Framework sprawdzi, czy te operacje są wykonywane w bazie danych.

Poniższy przykład pokazuje formant *GridView* zmodyfikowany, aby umożliwić sortowanie i stronicowanie:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample24.aspx)]

Teraz po uruchomieniu strony formant może upewnić się, że jest wyświetlana tylko bieżąca strona danych i że jest ona uporządkowana według zaznaczonej kolumny:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.png)

Aby odfiltrować zwrócone dane, należy dodać parametry do metody Select. Te parametry zostaną wypełnione przez powiązanie modelu w czasie wykonywania i można użyć ich do zmiany zapytania przed zwróceniem danych.

Załóżmy na przykład, że chcesz zezwolić użytkownikom na filtrowanie produktów przez wprowadzenie słowa kluczowego w ciągu zapytania. Można dodać parametr do metody i zaktualizować kod, aby użyć wartości parametru:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample25.cs)]

Ten kod zawiera wyrażenie *WHERE* , jeśli wartość jest podana dla *słowa kluczowego* , a następnie zwraca wyniki zapytania.

<a id="_Toc318097389"></a>
#### <a name="value-providers"></a>Dostawcy wartości

Poprzedni przykład nie był specyficzny dla miejsca, z którego pochodzi wartość parametru *słowa kluczowego* . Aby wskazać te informacje, można użyć atrybutu parametru. Na potrzeby tego przykładu można użyć klasy *QueryStringAttribute* , która znajduje się w przestrzeni nazw *System. Web. ModelBinding* :

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample26.cs)]

Powoduje to, że powiązanie modelu próbuje powiązać wartość z ciągu zapytania z parametrem *słowa kluczowego* w czasie wykonywania. (Może to dotyczyć konwersji typów, chociaż nie jest w tym przypadku). Jeśli nie można podać wartości, a typ nie dopuszcza wartości null, zgłaszany jest wyjątek.

Źródła wartości dla tych metod są określane jako dostawcy wartości i atrybuty parametrów wskazujące, który dostawca wartości, który ma być używany, jest określany jako atrybuty dostawcy wartości. Formularze sieci Web będą obejmować dostawców wartości i odpowiadające im atrybuty dla wszystkich typowych źródeł danych wejściowych użytkownika w aplikacji formularzy sieci Web, takich jak ciąg zapytania, pliki cookie, wartości formularzy, kontrolki, stan widoku, stan sesji i właściwości profilu. Możesz również pisać dostawców wartości niestandardowych.

Domyślnie nazwa parametru jest używana jako klucz do znajdowania wartości w kolekcji dostawców wartości. W przykładzie kod będzie szukać wartości ciągu zapytania o nazwie słowo kluczowe (na przykład ~/Default.aspx? słowo kluczowe = Chef). Możesz określić klucz niestandardowy przez przekazanie go jako argumentu do atrybutu Parameter. Na przykład, aby użyć wartości zmiennej ciągu zapytania o nazwie q, można to zrobić:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample27.cs)]

Jeśli ta metoda znajduje się w kodzie strony, użytkownicy mogą filtrować wyniki przez przekazanie słowa kluczowego przy użyciu ciągu zapytania:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.png)

Powiązanie modelu wykonuje wiele zadań, które w przeciwnym razie trzeba będzie zakodować przez ręczne: Odczytywanie wartości, sprawdzanie wartości null, próba konwersji na odpowiedni typ, sprawdzanie, czy konwersja zakończyła się powodzeniem, a wreszcie przy użyciu wartości w dotyczących. Powiązanie modelu skutkuje znacznie mniejszą ilością kodu i możliwością ponownego użycia funkcji w aplikacji.

<a id="_Toc318097390"></a>
#### <a name="filtering-by-values-from-a-control"></a>Filtrowanie według wartości z kontrolki

Załóżmy, że chcesz zwiększyć przykład, aby pozwolić użytkownikowi na wybranie wartości filtru z listy rozwijanej. Dodaj następującą listę rozwijaną do znacznika i skonfiguruj ją, aby pobrać dane z innej metody przy użyciu właściwości *SelectMethod* :

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample28.aspx)]

Zwykle do kontrolki *GridView* zostanie dodany element *EmptyDataTemplate* , dzięki czemu kontrolka wyświetli komunikat, jeśli nie zostaną znalezione pasujące produkty:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample29.aspx)]

W kodzie strony Dodaj nową metodę select dla listy rozwijanej:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample30.cs)]

Na koniec należy zaktualizować metodę " *Getproductss* Select", aby pobrać nowy parametr zawierający identyfikator wybranej kategorii z listy rozwijanej:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample31.cs)]

Gdy strona zostanie uruchomiona, użytkownicy mogą wybrać kategorię z listy rozwijanej, a formant *GridView* jest automatycznie ponownie powiązany, aby pokazać filtrowane dane. Jest to możliwe, ponieważ powiązanie modelu śledzi wartości parametrów dla metody Select i wykrywa, czy jakakolwiek wartość parametru została zmieniona po odświeżeniu. Jeśli tak, powiązanie modelu wymusza, aby skojarzona kontrolka danych mogła ponownie powiązać dane.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.png)

<a id="_Toc318097391"></a>
### <a name="html-encoded-data-binding-expressions"></a>Wyrażenia wiązania danych kodowane w formacie HTML

Teraz możesz kodować kod HTML z wynikami wyrażeń powiązań danych. Dodaj dwukropek (:) na końcu prefiksu &lt;% #, który oznacza wyrażenie powiązania danych:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample32.aspx)]

<a id="_Toc318097392"></a>
### <a name="unobtrusive-validation"></a>Niezauważalna weryfikacja

Teraz można skonfigurować wbudowane kontrolki Walidatora, aby użyć niewygodnego języka JavaScript dla logiki walidacji po stronie klienta. Znacznie zmniejsza to ilość kodu JavaScript renderowanego wewnętrznie w znaczniku strony i zmniejsza całkowity rozmiar strony. Można skonfigurować niedyskretny kod JavaScript dla kontrolek sprawdzania poprawności w dowolny z następujących sposobów:

- Globalnie przez dodanie następującego ustawienia do *&lt;appSettings&gt;* elementu w pliku Web. config: 

    [!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample33.xml)]
- Globalnie ustawiając właściwość static *System. Web. UI. właściwość. UnobtrusiveValidationMode* na *UnobtrusiveValidationMode. WebForms* (zazwyczaj w *aplikacji\_Start* w pliku Global. asax).
- Pojedynczo dla strony, ustawiając nową właściwość *UnobtrusiveValidationMode* klasy *Page* na *UnobtrusiveValidationMode. WebForms*.

<a id="_Toc318097393"></a>
### <a name="html5-updates"></a>Aktualizacje HTML5

Wprowadzono kilka ulepszeń w kontrolkach serwera formularzy sieci Web, aby korzystać z nowych funkcji języka HTML5:

- Właściwość *TextMode* kontrolki *TextBox* została zaktualizowana w celu obsługi nowych typów danych wejściowych HTML5, takich jak *wiadomości e-mail*, *DateTime*i tak dalej.
- Kontrolka *FileUpload* obsługuje teraz wiele operacji przekazywania plików z przeglądarek, które obsługują tę funkcję HTML5.
- Kontrolki walidatora obsługują teraz sprawdzanie poprawności elementów wejściowych HTML5.
- Nowe elementy HTML5, które mają atrybuty reprezentujące adres URL, obsługują teraz runat = "Server". W związku z tym można używać konwencji ASP.NET w ścieżkach URL, takich jak operator ~, aby reprezentować katalog główny aplikacji (na przykład &lt;Video runat = "Server" src = "~/myVideo.wmv"/&gt;).
- Kontrolka *UpdatePanel* została naprawiona w celu obsługi publikowania pól wejściowych HTML5.

<a id="_Toc318097394"></a>
## <a name="aspnet-mvc-4"></a>ASP.NET MVC w wersji 4

ASP.NET MVC 4 beta jest teraz dołączony do programu Visual Studio 11 beta. ASP.NET MVC to platforma służąca do opracowywania wysoce weryfikowalne i konserwowanych aplikacji sieci Web, wykorzystując wzorzec Model-View-Controller (MVC). ASP.NET MVC 4 ułatwia tworzenie aplikacji dla mobilnej sieci Web i zawiera internetowy interfejs API ASP.NET, który pomaga tworzyć usługi HTTP, które mogą dotrzeć do dowolnego urządzenia. Aby uzyskać więcej informacji, zobacz [Informacje o wersji ASP.NET MVC 4](mvc4-release-notes.md).

<a id="_Toc318097395"></a>
## <a name="aspnet-web-pages-2"></a>Strony sieci Web programu ASP.NET 2

Dostępne są następujące nowe funkcje:

- Nowe i zaktualizowane szablony witryn.
- Dodawanie walidacji po stronie serwera i po stronie klienta za pomocą pomocnika *walidacji* .
- Możliwość rejestrowania skryptów przy użyciu Menedżera zasobów.
- Włączanie logowania z serwisu Facebook i innych witryn przy użyciu protokołu OAuth i OpenID Connect.
- Dodawanie map przy użyciu pomocnika *Maps* .
- Uruchamianie aplikacji stron sieci Web obok siebie.
- Renderowanie stron dla urządzeń przenośnych.

Aby uzyskać więcej informacji na temat tych funkcji i przykładów kodu pełnej strony, zobacz [najważniejsze funkcje w stronach Web Pages 2 beta](https://go.microsoft.com/fwlink/?LinkID=227824).

<a id="_Toc318097396"></a>
## <a name="visual-web-developer-11-beta"></a>Visual Web Developer 11 beta

Ta sekcja zawiera informacje o ulepszeniach programowania w sieci Web w programie Visual Web Developer 11 beta i programie Visual Studio 2012 Release Candidate.

<a id="project-compatibility"></a>
### <a name="project-sharing-between-visual-studio-2010-and-visual-studio-2012-release-candidate-project-compatibility"></a>Udostępnianie projektu między programem Visual Studio 2010 i Visual Studio 2012 Release Candidate (zgodność projektu)

Dopóki program Visual Studio 2012 Release Candidate otworzy istniejący projekt w nowszej wersji programu Visual Studio, zostanie uruchomiony Kreator konwersji. To wymuszone uaktualnienie zawartości projektu i rozwiązania do nowych formatów, które nie są zgodne z poprzednimi wersjami. W związku z tym po konwersji nie można otworzyć projektu w starszej wersji programu Visual Studio.

Wielu klientów poinformowało nas, że to nie było właściwe podejście. W programie Visual Studio 11 beta teraz obsługujemy Udostępnianie projektów i rozwiązań w programie Visual Studio 2010 z dodatkiem SP1. Oznacza to, że w przypadku otwarcia projektu 2010 w programie Visual Studio 2012 Release Candidate nadal będzie można otworzyć projekt w programie Visual Studio 2010 SP1.

> [!NOTE]
> Nie można współużytkować kilku typów projektów między programem Visual Studio 2010 z dodatkiem SP1 a programem Visual Studio 2012 Release Candidate. Obejmują one niektóre starsze projekty (na przykład ASP.NET MVC 2) lub projekty do celów specjalnych (na przykład projekty instalacji).

Po otwarciu projektu sieci Web programu Visual Studio 2010 z dodatkiem SP1 po raz pierwszy w programie Visual Studio 11 Beta do pliku projektu dodawane są następujące właściwości:

- FileUpgradeFlags
- UpgradeBackupLocation
- OldToolsVersion
- VisualStudioVersion
- VSToolsPath

FileUpgradeFlags, UpgradeBackupLocation i OldToolsVersion są używane przez proces, który uaktualnia plik projektu. Nie mają one wpływu na pracę z projektem w programie Visual Studio 2010.

VisualStudioVersion to nowa właściwość używana przez MSBuild 4,5, która wskazuje wersję programu Visual Studio dla bieżącego projektu. Ponieważ ta właściwość nie istnieje w programie MSBuild 4,0 (wersja programu MSBuild, z której korzysta program Visual Studio 2010 SP1), wprowadzamy wartość domyślną do pliku projektu.

Właściwość VSToolsPath służy do określenia poprawnego pliku TARGETS do zaimportowania ze ścieżki reprezentowanej przez ustawienie MSBuildExtensionsPath32.

Istnieją także pewne zmiany związane z elementami importu. Te zmiany są wymagane w celu zapewnienia zgodności między obiema wersjami programu Visual Studio.

> [!NOTE]
> Jeśli projekt jest współużytkowany przez program Visual Studio 2010 z dodatkiem SP1 i program Visual Studio 11 beta na dwóch różnych komputerach, a projekt zawiera lokalną bazę danych w folderze danych\_aplikacji, należy upewnić się, że wersja SQL Server używana przez bazę danych jest zainstalowana na obu komputerach.

<a id="Configuration_Changes_In_ASPNET45_Website_Templates"></a>
### <a name="configuration-changes-in-aspnet-45-website-templates"></a>Zmiany konfiguracji w szablonach witryny sieci Web ASP.NET 4,5

Następujące zmiany zostały wprowadzone w pliku default *Web. config* dla witryny utworzonej przy użyciu szablonów witryn sieci Web w programie Visual Studio 2012 Release Candidate:

- W elemencie `<httpRuntime>` atrybut `encoderType` jest teraz ustawiany domyślnie, aby używać typów AntiXSS, które zostały dodane do ASP.NET. Aby uzyskać szczegółowe informacje, zobacz [Biblioteka AntiXSS](#_Toc318097382).
- Również w elemencie `<httpRuntime>` atrybut `requestValidationMode` ma wartość "4,5". Oznacza to, że domyślnie sprawdzanie poprawności żądania jest skonfigurowane do korzystania z odroczonego ("opóźnionego") walidacji. Aby uzyskać szczegółowe informacje, zobacz [New ASP.NET Request Validation Features](#_Toc318097379).
- Element `<modules>` sekcji `<system.webServer>` nie zawiera atrybutu `runAllManagedModulesForAllRequests`. (Wartość domyślna to false). Oznacza to, że w przypadku korzystania z wersji programu IIS 7, która nie została zaktualizowana do programu SP1, mogą wystąpić problemy z routingiem w nowej lokacji. Aby uzyskać więcej informacji, zobacz [Natywne wsparcie w usługach IIS 7 dla routingu ASP.NET](#Native_Support_In_IIS7_For_ASPNET_Routine).

Te zmiany nie mają wpływu na istniejące aplikacje. Jednak mogą one reprezentować różnice w zachowaniu między istniejącymi witrynami sieci Web i nowymi witrynami sieci Web, które tworzysz dla ASP.NET 4,5 przy użyciu nowych szablonów.

<a id="Native_Support_In_IIS7_For_ASPNET_Routine"></a>
### <a name="native-support-in-iis-7-for-aspnet-routing"></a>Natywna obsługa w usługach IIS 7 dla routingu ASP.NET

Nie jest to zmiana ASP.NET w taki sposób, ale zmiana szablonów dla nowych projektów witryny sieci Web, które mogą mieć wpływ na użytkownika w przypadku korzystania z wersji programu IIS 7, w której nie zastosowano aktualizacji z dodatkiem SP1.

W programie ASP.NET można dodać do aplikacji następujące ustawienia konfiguracji, aby umożliwić obsługę routingu:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample34.xml?highlight=3)]

Gdy **runAllManagedModulesForAllRequests** ma wartość true, adres URL, taki jak `http://mysite/myapp/home`, prowadzi do ASP.NET, nawet jeśli nie istnieje rozszerzenie *aspx*, *MVC*lub podobne.

Aktualizacja wprowadzona w usługach IIS 7 sprawia, że ustawienie **runAllManagedModulesForAllRequests** jest niepotrzebne i obsługuje natywne Routing ASP.NET. (Aby uzyskać informacje na temat aktualizacji, zobacz artykuł pomoc techniczna firmy Microsoft [jest dostępna aktualizacja umożliwiająca niektórym usługom iis 7,0 lub iis 7,5 obsługę żądań, których adresy URL nie kończą się kropką](https://support.microsoft.com/kb/980368).)

Jeśli witryna sieci Web jest uruchomiona w usługach IIS 7, a usługi IIS zostały zaktualizowane, nie musisz ustawiać **runAllManagedModulesForAllRequests** na true. W rzeczywistości ustawienie tego ustawienia na wartość true nie jest zalecane, ponieważ powoduje dodanie niepotrzebnego obciążenia przetwarzania do żądania. Gdy to ustawienie ma wartość true, wszystkie żądania, w tym te dla plików *htm*, *jpg*i inne pliki statyczne, również przechodzą przez potok żądania ASP.NET.

Jeśli utworzysz nową witrynę sieci Web ASP.NET 4,5 przy użyciu szablonów, które są dostępne w programie Visual Studio 2012 RC, konfiguracja witryny sieci Web nie obejmuje ustawienia **runAllManagedModulesForAllRequests** . Oznacza to, że domyślnie ustawienie ma wartość false.

Po uruchomieniu witryny sieci Web w systemie Windows 7 bez dodatku SP1 usługi IIS 7 nie będą zawierać wymaganej aktualizacji. W związku z tym routing nie będzie działał i zobaczysz błędy. Jeśli wystąpi problem, którego Routing nie działa, można wykonać następujące czynności:

- Zaktualizuj system Windows 7 do wersji SP1, co spowoduje dodanie aktualizacji do usług IIS 7.
- Zainstaluj aktualizację, która została opisana w wcześniej wymienionym artykule pomoc techniczna firmy Microsoft.
- Ustaw **runAllManagedModulesForAllRequests** na wartość true w pliku Web. config tej witryny sieci Web. Należy zauważyć, że spowoduje to dodanie pewnej nakładu pracy na żądania.

<a id="_Toc318097397"></a>
### <a name="html-editor"></a>{1&gt;Edytor HTML&lt;1}

<a id="_Toc318097398"></a>
#### <a name="smart-tasks"></a>Zadania inteligentne

W widok Projekt złożone właściwości formantów serwera często mają skojarzone okna dialogowe i kreatory, aby ułatwić ich ustawianie. Na przykład możesz użyć specjalnego okna dialogowego, aby dodać źródło danych do kontrolki *wzmacniak* lub dodać kolumny do kontrolki *GridView* .

Jednak ten typ pomocy interfejsu użytkownika dla właściwości złożonych nie jest dostępny w widoku źródła. W związku z tym program Visual Studio 11 wprowadza inteligentne zadania dla widoku źródła. Inteligentne zadania to skróty z obsługą kontekstu dla często używanych funkcji w C# edytorach i Visual Basic.

W przypadku formantów ASP.NET Web Forms zadania inteligentne są wyświetlane w tagach serwera jako mały symbol, gdy punkt wstawiania znajduje się wewnątrz elementu:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.png)

Panel inteligentny rozwija się po kliknięciu glifu lub naciśnięciu klawiszy CTRL +. (kropka), podobnie jak w edytorach kodu. Następnie są wyświetlane skróty podobne do inteligentnych zadań w widok Projekt.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image7.png)

Na przykład panel inteligentny na poprzedniej ilustracji przedstawia opcje zadań GridView. W przypadku wybrania opcji Edytuj kolumny zostanie wyświetlone następujące okno dialogowe:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image8.png)

Wypełnienie okna dialogowego ustawia te same właściwości, które można ustawić w widok Projekt. Po kliknięciu przycisku OK, znaczniki dla kontrolki zostaną zaktualizowane o nowe ustawienia:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image9.png)

<a id="_Toc318097399"></a>
#### <a name="wai-aria-support"></a>Obsługa poczeka — ARIA

Pisanie dostępnych witryn sieci Web staje się coraz bardziej ważne. [Standard dostępności poczeka-Aria](http://www.w3.org/WAI/intro/aria) definiuje, w jaki sposób deweloperzy powinni pisać dostępne witryny sieci Web. Ten standard jest teraz w pełni obsługiwany w programie Visual Studio.

Na przykład atrybut *role* ma teraz pełną funkcję IntelliSense:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image10.png)

W standardzie poczeka-ARIA wprowadzono również atrybuty, które są poprzedzone znakiem *Aria* , dzięki czemu można dodać semantykę do dokumentu HTML5. Program Visual Studio również w pełni obsługuje te *Aria* atrybuty:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image11.png) ![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image12.png)

<a id="_Toc318097400"></a>
#### <a name="new-html5-snippets"></a>Nowe fragmenty kodu HTML5

Aby szybciej i łatwiej pisać często używane znaczniki HTML5, program Visual Studio zawiera kilka fragmentów kodu. Przykładem jest fragment wideo:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image13.png)

Aby wywołać fragment kodu, naciśnij dwukrotnie klawisz Tab, gdy element jest zaznaczony w IntelliSense:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image14.png)

Spowoduje to utworzenie fragmentu, który można dostosować.

<a id="_Toc318097401"></a>
#### <a name="extract-to-user-control"></a>Wyodrębnij do kontrolki użytkownika

W dużych stronach sieci Web może być dobrym pomysłem przenieść poszczególne elementy do kontrolek użytkownika. Ta forma refaktoryzacji może pomóc zwiększyć czytelność strony i uprościć strukturę strony.

Aby to ułatwić, podczas edytowania stron formularzy sieci Web w widoku źródła można teraz zaznaczyć tekst na stronie, kliknąć go prawym przyciskiem myszy, a następnie wybrać polecenie Wyodrębnij do kontrolki użytkownika:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.jpg)

<a id="_Toc318097402"></a>
#### <a name="intellisense-for-code-nuggets-in-attributes"></a>Funkcja IntelliSense dla Nuggets kodu w atrybutach

Program Visual Studio zawsze udostępnia funkcję IntelliSense dla kodu po stronie serwera Nuggets na dowolnej stronie lub kontrolce. Teraz program Visual Studio zawiera również funkcję IntelliSense dla kodu Nuggets w atrybutach HTML.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image15.png)

Ułatwia to tworzenie wyrażeń powiązań danych:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image16.png)

<a id="_Toc318097403"></a>
#### <a name="automatic-renaming-of-matching-tag-when-you-rename-an-opening-or-closing-tag"></a>Automatyczna zmiana nazwy pasującego tagu w przypadku zmiany nazwy otwierającego lub zamykającego tagu

Jeśli zmienisz nazwę elementu HTML (na przykład zmienisz tag *DIV* jako tag *nagłówka* ), odpowiedni tag otwierający lub zamykający również zmienia się w czasie rzeczywistym.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image17.png)

Pozwala to uniknąć błędów, w których zapomnisz zmienić tag zamykający lub zmienić niewłaściwy.

<a id="_Toc318097404"></a>
#### <a name="event-handler-generation"></a>Generowanie programu obsługi zdarzeń

Program Visual Studio zawiera teraz funkcje w widoku źródła ułatwiające pisanie programów obsługi zdarzeń i Powiązywanie ich ręcznie. Jeśli edytujesz nazwę zdarzenia w widoku źródła, IntelliSense wyświetla &lt;Utwórz nowe zdarzenie&gt;, co spowoduje utworzenie programu obsługi zdarzeń w kodzie strony, który ma prawidłowy podpis:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.jpg)

Domyślnie program obsługi zdarzeń będzie używać identyfikatora kontrolki dla nazwy metody obsługi zdarzeń:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.jpg)

Program obsługi zdarzeń będzie wyglądać następująco (w tym przypadku w C#):

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image18.png)

<a id="_Toc318097405"></a>
#### <a name="smart-indent"></a>Inteligentne wcięcie

Po naciśnięciu klawisza ENTER w obrębie pustego elementu HTML Edytor umieści punkt wstawiania w odpowiednim miejscu:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image19.png)

Po naciśnięciu klawisza ENTER w tej lokalizacji tag zamykający zostanie przeniesiony w dół i zostanie wcięty do znacznika otwierającego. Punkt wstawiania jest również wcięty:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image20.png)

<a id="_Toc318097406"></a>
#### <a name="auto-reduce-statement-completion"></a>Autouzupełnianie uzupełniania instrukcji

Lista funkcji IntelliSense w programie Visual Studio teraz filtruje się na podstawie tego, co zostało wpisane, aby wyświetlić tylko odpowiednie opcje:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image21.png)

Technologia IntelliSense również filtruje w zależności od wielkości liter poszczególnych wyrazów na liście IntelliSense. Na przykład po wpisaniu "DL" są wyświetlane zarówno DL, jak i ASP: DataList:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image22.png)

Ta funkcja przyspiesza pobieranie instrukcji dla znanych elementów.

<a id="_Toc318097407"></a>
### <a name="javascript-editor"></a>JavaScript — Edytor

Edytor JavaScript w programie Visual Studio 2012 Release Candidate jest zupełnie nowy i znacznie poprawia środowisko pracy z JavaScript w programie Visual Studio.

<a id="_Toc318097408"></a>
#### <a name="code-outlining"></a>Konspekt kodu

Regiony tworzenia konspektu są teraz automatycznie tworzone dla wszystkich funkcji, co pozwala na zwijanie części pliku, które nie są związane z bieżącym fokusem.

<a id="_Toc318097409"></a>
#### <a name="brace-matching"></a>Dopasowywanie nawiasów klamrowych

Po umieszczeniu punktu wstawiania w otwierającym lub zamykającym nawiasie klamrowym Edytor wyróżni pasujący jeden.

<a id="_Toc318097410"></a>
#### <a name="go-to-definition"></a>Przejdź do definicji

Polecenie Przejdź do definicji umożliwia przechodzenie do źródła funkcji lub zmiennej.

<a id="_Toc318097411"></a>
#### <a name="ecmascript5-support"></a>Obsługa ECMAScript5

Edytor obsługuje nową składnię i interfejsy API w ECMAScript5, najnowszą wersję standardu opisującą język JavaScript.

<a id="_Toc318097412"></a>
#### <a name="dom-intellisense"></a>Funkcja IntelliSense modelu DOM

Ulepszono technologię IntelliSense dla interfejsów API języka DOM, z obsługą wielu nowych interfejsów API HTML5, takich jak *querySelector*, dom Storage, obsługa komunikatów między dokumentami i *kanwy*. Technologia IntelliSense modelu DOM jest teraz oparta na jednym prostym pliku JavaScript, a nie w definicji biblioteki typów natywnych. Pozwala to łatwo rozciągnąć lub zamienić.

<a id="_Toc318097413"></a>
#### <a name="vsdoc-signature-overloads"></a>Przeciążenia sygnatur VSDOC

Szczegółowe komentarze IntelliSense można teraz zadeklarować dla oddzielnych przeciążeń funkcji JavaScript przy użyciu nowego *&lt;sygnatury&gt;* elementu, jak pokazano w tym przykładzie:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample35.cs)]

<a id="_Toc318097414"></a>
#### <a name="implicit-references"></a>Niejawne odwołania

Możesz teraz dodawać pliki JavaScript do centralnej listy, która zostanie niejawnie uwzględniona na liście plików, do których odwołuje się każdy plik JavaScript lub blok, co oznacza, że w jego zawartości uzyskasz IntelliSense. Na przykład można dodać pliki jQuery do centralnej listy plików, a uzyskasz funkcję IntelliSense dla jQuery w dowolnym bloku JavaScript pliku, niezależnie od tego, czy odwołujesz się do niego jawnie (przy użyciu///&lt;odwołanie/&gt;).

<a id="_Toc318097415"></a>
### <a name="css-editor"></a>Edytor CSS

<a id="_Toc318097416"></a>
#### <a name="auto-reduce-statement-completion"></a>Autouzupełnianie uzupełniania instrukcji

Lista funkcji IntelliSense dla filtrów CSS teraz oparta na właściwościach i wartościach CSS obsługiwanych przez wybrany schemat.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image23.png)

Technologia IntelliSense obsługuje również wyszukiwanie wielkości liter w tytule:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image24.png)

<a id="_Toc318097417"></a>
#### <a name="hierarchical-indentation"></a>Wcięcie hierarchiczne

Edytor CSS używa wcięć do wyświetlania reguł hierarchicznych, co zapewnia przegląd logicznie zorganizowanych reguł kaskadowych. W poniższym przykładzie #list selektorem jest kaskadowy element podrzędny listy i w związku z tym jest wcięty.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image25.png)

Poniższy przykład pokazuje bardziej złożone dziedziczenie:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image26.png)

Wcięcia reguły są określane przez reguły nadrzędne. Wcięcia hierarchiczne są domyślnie włączone, ale można je wyłączyć za pomocą okna dialogowego Opcje (narzędzia, opcje z paska menu):

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image27.png)

<a id="_Toc318097418"></a>
#### <a name="css-hacks-support"></a>Obsługa CSS Hacks

Analiza setek plików CSS w rzeczywistości pokazuje, że CSS Hacks jest bardzo powszechny, a teraz program Visual Studio obsługuje najczęściej używane. Ta obsługa obejmuje IntelliSense i weryfikację właściwości gwiazdki (\*) i podkreślenia (\_) Hacks:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image28.png)

Typowe Hacks selektora są również obsługiwane, aby wcięcia hierarchiczne były utrzymywane nawet wtedy, gdy są stosowane. Typowym hakerem selektora używanym do celów programu Internet Explorer 7 jest dołączanie selektora z *\*: pierwszy-podrzędny + HTML*. Użycie tej reguły spowoduje zachowanie wcięcia hierarchicznego:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image29.png)

<a id="_Toc318097419"></a>
#### <a name="vendor-specific-schemas--moz---webkit"></a>Schematy specyficzne dla dostawcy (-moz-,-WebKit)

CSS3 wprowadza wiele właściwości, które zostały zaimplementowane przez różne przeglądarki w różnym czasie. Ten wcześniej wymuszony deweloperzy do kodu dla określonych przeglądarek przy użyciu składni specyficznej dla dostawcy. Te właściwości specyficzne dla przeglądarki są teraz zawarte w technologii IntelliSense.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image30.png)

<a id="_Toc318097420"></a>
#### <a name="commenting-and-uncommenting-support"></a>Obsługa komentowania i usuwania komentarzy

Możesz teraz komentować i odkomentować reguły CSS przy użyciu tych samych skrótów, które są używane w edytorze kodu (Ctrl + K, C, aby skomentować i CTRL + K, możesz usunąć komentarz).

<a id="_Toc318097421"></a>
#### <a name="color-picker"></a>Selektor kolorów

W poprzednich wersjach programu Visual Studio funkcja IntelliSense dla atrybutów związanych z kolorem zawierała listę rozwijaną o nazwanych wartościach kolorów. Ta lista została zastąpiona przez pełny wybór kolorów.

Po wprowadzeniu wartości koloru próbnik koloru jest wyświetlany automatycznie i przedstawia listę poprzednio używanych kolorów, po których następuje domyślna paleta kolorów. Możesz wybrać kolor przy użyciu myszy lub klawiatury.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image31.png)

Listę można rozwinąć do pełnego selektora kolorów. Selektor umożliwia kontrolowanie kanału alfa przez automatyczne konwertowanie dowolnego koloru na RGBA po przesunięciu suwaka nieprzezroczystość:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image32.png)

<a id="_Toc318097422"></a>
#### <a name="snippets"></a>Fragmenty kodu

Fragmenty kodu w edytorze CSS ułatwiają i przyspieszają tworzenie stylów między przeglądarkami. Wiele właściwości CSS3, które wymagają ustawień specyficznych dla przeglądarki, jest teraz rzutowane na fragmenty kodu.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image33.png)

Fragmenty kodu CSS obsługują zaawansowane scenariusze (na przykład zapytania o multimedia CSS3), wpisując symbol w znakach (@), który wyświetla listę funkcji IntelliSense.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image34.png)

Po wybraniu opcji @media wartość i naciśnięciu klawisza Tab Edytor CSS wstawia następujący fragment kodu:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.jpg)

Podobnie jak w przypadku fragmentów kodu, można tworzyć własne wstawki CSS.

<a id="_Toc318097423"></a>
#### <a name="custom-regions"></a>Regiony niestandardowe

Regiony kodu nazwane, które są już dostępne w edytorze kodu, są teraz dostępne do edycji CSS. Pozwala to łatwo grupować powiązane bloki stylów.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image35.png)

Gdy region jest zwinięty, wyświetlana jest nazwa regionu:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image36.png)

<a id="_Toc318097424"></a>
### <a name="page-inspector"></a>{1&gt;Inspektor strony&lt;1}

Page Inspector to narzędzie, które renderuje stronę sieci Web (HTML, Web Forms, ASP.NET MVC lub Web Pages) w środowisku IDE programu Visual Studio i umożliwia badanie kodu źródłowego oraz wynikowych danych wyjściowych. W przypadku stron ASP.NET Inspektor strony pozwala określić, który kod po stronie serwera wygenerował znaczniki HTML, które są renderowane w przeglądarce.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image37.png)

Aby uzyskać więcej informacji na temat inspektora stron, zobacz następujące samouczki:

- Korzystanie z narzędzia Page Inspector w [ASP.NET MVC](../mvc/overview/views/using-page-inspector-in-aspnet-mvc.md)
- Korzystanie z narzędzia Page Inspector w [ASP.NET Web Forms](../web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project.md)

<a id="_Toc318097425"></a>
### <a name="publishing"></a>Publikowanie

<a id="_Toc318097426"></a>
#### <a name="publish-profiles"></a>Publikowanie profilów

W programie Visual Studio 2010 informacje o publikowaniu dla projektów aplikacji sieci Web nie są przechowywane w kontroli wersji i nie są przeznaczone do udostępniania innym osobom. W programie Visual Studio 2012 Release Candidate format profilu publikowania został zmieniony. Został złożony artefakt zespołu i teraz można łatwo korzystać z kompilacji opartych na programie MSBuild. Informacje o konfiguracji kompilacji są w oknie dialogowym Publikowanie, dzięki czemu można łatwo przełączyć konfiguracje kompilacji przed opublikowaniem.

Profile publikowania są przechowywane w folderze PublishProfiles. Lokalizacja folderu zależy od używanego języka programowania:

- C#: Properties\PublishProfiles
- Visual Basic: moje Project\PublishProfiles

Każdy profil jest plikiem programu MSBuild. Podczas publikowania ten plik jest zaimportowany do pliku MSBuild projektu. W programie Visual Studio 2010, jeśli chcesz wprowadzić zmiany w procesie publikowania lub tworzenia pakietu, musisz umieścić dostosowania w pliku o nazwie **ProjectName**. WPP. targets. Jest to nadal obsługiwane, ale teraz można umieścić dostosowania w samym profilu publikacji. Dzięki temu dostosowania będą używane tylko dla tego profilu.

Teraz możesz również korzystać z profilów publikowania z programu MSBuild. Aby to zrobić, użyj następującego polecenia podczas kompilowania projektu:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample36.cmd)]

Wartość Project. csproj jest ścieżką projektu, a nazwa pliku to nazwę profilu do opublikowania. Alternatywnie zamiast przekazywać nazwę profilu dla właściwości *pozycji publishprofile* , można przekazać pełną ścieżkę do profilu publikacji.

<a id="_Toc318097427"></a>
#### <a name="aspnet-precompilation-and-merge"></a>ASP.NET prekompilowania i scalania

W przypadku projektów aplikacji sieci Web program Visual Studio 2012 Release Candidate dodaje opcję na stronie pakowanie/publikowanie właściwości sieci Web, która umożliwia wstępne Kompilowanie i scalanie zawartości witryny podczas publikowania lub pakowania projektu. Aby wyświetlić te opcje, kliknij prawym przyciskiem myszy projekt w Eksplorator rozwiązań, wybierz polecenie Właściwości, a następnie wybierz stronę właściwości pakowanie/publikowanie w sieci Web. Na poniższej ilustracji przedstawiono opcję prekompilowania tej aplikacji przed opublikowaniem.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.jpg)

Gdy ta opcja jest zaznaczona, program Visual Studio prekompiluje aplikację po każdym opublikowaniu lub spakowaniu aplikacji sieci Web. Aby kontrolować sposób, w jaki witryna jest wstępnie skompilowana lub jak zestawy są scalane, kliknij przycisk Zaawansowane, aby skonfigurować te opcje.

<a id="_Toc318097428"></a>
### <a name="iis-express"></a>Usługi IIS Express

Domyślny serwer sieci Web do testowania projektów sieci Web w programie Visual Studio jest teraz IIS Express. Serwer programistyczny programu Visual Studio jest nadal opcją dla lokalnego serwera sieci Web podczas opracowywania, ale IIS Express jest teraz zalecanym serwerem. Środowisko korzystania z IIS Express w programie Visual Studio 11 Beta jest bardzo podobne do użycia w programie Visual Studio 2010 z dodatkiem SP1.

<a id="_Toc318097429"></a>
## <a name="disclaimer"></a>Zastrzeżenie

Ten dokument jest wstępny i może ulec zmianie w odróżnieniu od ostatecznej wersji handlowej oprogramowania opisanego w niniejszej umowie.

Informacje zawarte w tym dokumencie przedstawiają bieżący widok firmy Microsoft Corporation dotyczącej omawianych problemów w dniu publikacji. Ponieważ firma Microsoft musi reagować na zmieniające się warunki rynkowe, nie powinna być interpretowana jako zobowiązanie w ramach firmy Microsoft, a firma Microsoft nie gwarantuje dokładności informacji przedstawionych po dacie publikacji.

Ten oficjalny dokument jest przeznaczony wyłącznie do celów informacyjnych. FIRMA MICROSOFT NIE UDZIELA ŻADNYCH GWARANCJI, WYRAŹNYCH, DOROZUMIANYCH ANI USTAWOWYCH, W ODNIESIENIU DO INFORMACJI ZAWARTYCH W TYM DOKUMENCIE.

Zgodnie ze wszystkimi obowiązującymi prawami autorskimi użytkownik jest odpowiedzialny za użytkownika. Bez ograniczania prawa w ramach praw autorskich, żadna część tego dokumentu nie może zostać odbudowana, zapisana lub wprowadzona w systemie pobierania ani przesłana w jakiejkolwiek formie ani w jakikolwiek sposób (elektroniczny, mechaniczny, z kopiowaniem, nagrań lub w inny sposób) lub w dowolnym celu. bez wyraźnej pisemnej zgody firmy Microsoft Corporation.

Firma Microsoft może mieć patenty, zgłoszenia patentowe, znaki towarowe, prawa autorskie lub inne prawa własności intelektualnej, w tym dokument. O ile nie zostało to wyraźnie określone w pisemnej umowie licencyjnej firmy Microsoft, dostarczenie tego dokumentu nie daje Licencjobiorcy żadnych licencji na te patenty, znaki towarowe, prawa autorskie lub inną własność intelektualną.

O ile nie zaznaczono inaczej, Przykładowe firmy, organizacje, produkty, nazwy domen, adresy e-mail, logo, osoby, miejsca i zdarzenia wymienione w tym dokumencie są fikcyjne i nie są powiązane z rzeczywistymi firmami, organizacjami, produktami, nazwami domen, adresami e-mail adres, logo, osoba, miejsce lub zdarzenie są zamierzone lub wywnioskowane.

© 2012 Microsoft Corporation. Wszelkie prawa zastrzeżone.

Microsoft i Windows są zastrzeżonymi znakami towarowymi lub znakami towarowymi firmy Microsoft Corporation w Stany Zjednoczone i/lub innych krajach.

Nazwy rzeczywistych firm i produktów wymienionych w niniejszym dokumencie mogą być znakami towarowymi odpowiednich właścicieli.

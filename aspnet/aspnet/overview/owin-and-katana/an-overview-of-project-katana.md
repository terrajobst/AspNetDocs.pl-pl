---
uid: aspnet/overview/owin-and-katana/an-overview-of-project-katana
title: Omówienie projektu Katana | Microsoft Docs
author: howarddierking
description: Platforma ASP.NET była około dziesięciu lat, a na platformie włączono tworzenie witryn i usług sieci Web niezliczone. Jako aplikacja sieci Web...
ms.author: riande
ms.date: 08/30/2013
ms.assetid: 0ee21741-c1bf-4025-a9b0-24580cae24bc
msc.legacyurl: /aspnet/overview/owin-and-katana/an-overview-of-project-katana
msc.type: authoredcontent
ms.openlocfilehash: 1f28db822930cdfd2ebf4cf9bb27d173f4aa4201
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78617237"
---
# <a name="an-overview-of-project-katana"></a>Omówienie projektu Katana

Autor [Howard Dierking](https://github.com/howarddierking)

> Platforma ASP.NET była około dziesięciu lat, a na platformie włączono tworzenie witryn i usług sieci Web niezliczone. Ponieważ strategie opracowywania aplikacji sieci Web uległy ewolucji, platforma mogła rozwijać się w kroku z technologiami takimi jak ASP.NET MVC i ASP.NET Web API. Ponieważ opracowywanie aplikacji sieci Web zajmie swój kolejny etap ewolucyjny w świecie przetwarzania danych w chmurze, program Project [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) udostępnia podstawowy zestaw składników do ASP.NET aplikacji, co umożliwia ich elastyczne, przenośne, lekkie i zapewnia lepszą wydajność — w inny sposób Usługa Project [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) cloud optymalizuje aplikacje ASP.NET.

## <a name="why-katana--why-now"></a>Dlaczego Katana — dlaczego to teraz?

 Bez względu na to, czy jedna z nich omawia strukturę dewelopera, czy produkt użytkownika końcowego, ważne jest zrozumienie podstawowych motywacji do tworzenia produktu — a częścią, która zawiera informacje o tym, kto został utworzony. Program ASP.NET został pierwotnie utworzony z dwoma klientami.   
  
**Pierwsza grupa klientów była klasycznymi deweloperami ASP.** W tej chwili środowisko ASP było jedną z podstawowych technologii tworzenia dynamicznych, opartych na danych witryn i aplikacji sieci Web dzięki obkorzystaniu znaczników i skryptów po stronie serwera. Środowisko uruchomieniowe ASP dostarczyło skrypt po stronie serwera z zestawem obiektów, które stanowią abstrakcyjne podstawowe aspekty bazowego protokołu HTTP i serwera sieci Web oraz zapewniają dostęp do dodatkowych usług takich jak sesja i zarządzanie stanem aplikacji, pamięć podręczna itd. Mimo że klasyczne aplikacje ASP stają się wyzwaniem do zarządzania w miarę zwiększania ich rozmiaru i złożoności. Było to duże ze względu na brak struktury w środowiskach skryptów, w połączeniu z duplikowaniem kodu wynikającego z przeplotu kodu i znaczników. Aby zmienić zalety klasycznego środowiska ASP podczas rozwiązywania niektórych wyzwań, ASP.NET korzystał z organizacji kodu dostarczonej przez Języki zorientowane obiektowo .NET Framework, jednocześnie zachowując model programowania po stronie serwera. do których deweloperzy klasycznych ASP uprawiali.

**Drugą grupą klientów docelowych dla ASP.NET były deweloperzy aplikacji systemu Windows dla firm.** W przeciwieństwie do klasycznych deweloperów ASP, którzy korzystali z pisania znaczników HTML i kodu w celu wygenerowania większej liczby znaczników HTML, deweloperzy programu WinForms (podobnie jak deweloperzy VB6 przed nimi) były przyzwyczajoni do środowiska projektowego, które zawiera kanwę i bogaty zestaw użytkowników kontrolki interfejsu. Pierwsza wersja ASP.NET — znana także jako "formularze sieci Web", która ma podobny czas projektowania wraz z modelem zdarzeń po stronie serwera dla składników interfejsu użytkownika i zestawu funkcji infrastruktury (takich jak stan wyświetlania), aby utworzyć bezproblemowe środowisko programistyczne między programowaniem po stronie klienta i serwera. Za pomocą formularzy sieci Web można skutecznie dołączać bezstanowy charakter sieci Web w ramach modelu zdarzeń stanowych, który był znany deweloperom WinForms.

### <a name="challenges-raised-by-the-historical-model"></a>Wyzwania zgłoszone przez model historyczny

**Wynikiem działania sieci jest przedwczesne, bogate w funkcje środowisko uruchomieniowe i model programowania dla deweloperów.** Jednak dzięki tej funkcji bogate możliwości zostały dołączone do kilku istotnych wyzwań. Po pierwsze, struktura była **lity**, z logicznie różnymi jednostkami funkcjonalności, które są ściśle powiązane z tym samym zestawem system. Web. dll (na przykład podstawowe obiekty http w strukturze formularzy sieci Web). Po drugie, ASP.NET został uwzględniony jako część większego .NET Framework, co oznacza, że **czas między wersjami był w kolejności lat.** Utrudnia to ASP.NET, aby zachować aktualność ze wszystkimi zmianami wykonywanymi w szybkim rozwoju aplikacji sieci Web. Wreszcie system. Web. dll został połączony na kilka różnych sposobów do konkretnej opcji hostingu w sieci Web: Internet Information Services (IIS).

### <a name="evolutionary-steps-aspnet-mvc-and-aspnet-web-api"></a>Kroki ewolucyjne: ASP.NET MVC i ASP.NET Web API

Wiele zmian zakończyło się w rozwoju aplikacji sieci Web! Aplikacje sieci Web były coraz bardziej opracowywane jako seria małych, ukierunkowanych składników, a nie dużych platform. Liczba składników, a także częstotliwość, z jaką zostały one wydane, zwiększa się w szybszym tempie. Było jasne, że utrzymywanie się w sieci Web wymagało, aby struktury były mniejsze, oddzielone i bardziej ukierunkowane, a nie większa i większa funkcja, dlatego **zespół ASP.NET musiał wykonać kilka kroków ewolucyjnych, aby włączyć ASP.NET jako rodzinę podłączanych składników sieci Web, a nie pojedynczej struktury**.

Jedną z wczesnych zmian była wzrost popularności dobrze znanego wzorca projektowego typu MVC (Model-View-Controller), dzięki czemu platformy programistyczne dla sieci Web, takie jak Ruby na szynach. Ten styl tworzenia aplikacji sieci Web umożliwił deweloperowi większą kontrolę nad znacznikiem aplikacji przy jednoczesnym zachowaniu rozdzielania znaczników i logiki biznesowej, które były jednym z początkowych punktów sprzedaży dla ASP.NET. Aby sprostać wymaganiom tego stylu opracowywania aplikacji sieci Web, firma Microsoft mogła lepiej ustalić swoją sytuację w przyszłości, **opracowując ASP.NET MVC poza pasmem** (bez uwzględniania go w .NET Framework). ASP.NET MVC został opublikowany jako niezależny do pobrania. Dzięki temu Zespół inżynieryjny elastycznie może dostarczać aktualizacje znacznie częściej niż wcześniej.

Kolejną istotną zmianą w tworzeniu aplikacji sieci Web była przechodzenie między dynamicznymi, generowanymi przez serwer stronami sieci Web a statycznymi znacznikami początkowymi z dynamicznymi sekcjami stron wygenerowanych przez skrypty po stronie klienta **z interfejsami API sieci Web zaplecza za pośrednictwem żądań AJAX**. Ta zmiana architektury pomogła Propel wzrostem poziomu interfejsów API sieci Web i programowaniem struktury internetowego interfejsu API ASP.NET. Tak jak w przypadku ASP.NET MVC, wydanie internetowego interfejsu API ASP.NET udostępnia kolejną okazję do rozwijania ASP.NET jako bardziej modularnego środowiska. Zespół inżynieryjny korzystał z możliwości i **skompilowanego interfejsu API sieci Web ASP.NET w taki sposób, że nie ma żadnych zależności od żadnego z typów podstawowych platformy znalezionych w pliku System. Web. dll**. Ta funkcja ma dwie rzeczy: najpierw to, że interfejs API sieci Web ASP.NET może się rozwijać w całkowicie niezależny sposób (i może kontynuować wielokrotnie, ponieważ jest dostarczany za pośrednictwem NuGet). Po drugie, ponieważ nie istniały żadne zależności zewnętrzne do System. Web. dll i w związku z tym nie ma żadnych zależności od usług IIS, interfejs API sieci Web ASP.NET dołączał możliwość uruchamiania na niestandardowym hoście (na przykład aplikacji konsolowej, usługi systemu Windows itp.)

### <a name="the-future-a-nimble-framework"></a>Przyszłość: elastyczna Framework

Łącząc składniki struktury ze sobą, a następnie zwalniając je w NuGet, platformy mogą teraz **wielokrotnie wykonywać iteracje i**szybciej. Ponadto moc i elastyczność możliwości samodzielnego udostępniania internetowego interfejsu API są bardzo atrakcyjne dla deweloperów, którzy chcąli **niewielki, lekki Host** dla swoich usług. Okazało się to atrakcyjne, w rzeczywistości, że inne struktury również potrzebowały tej możliwości, a to nowe wyzwanie polega na tym, że każda struktura działała w ramach własnego procesu hosta na własnym adresie podstawowym i musi być zarządzana (uruchomiona, zatrzymana itp.). Nowoczesne aplikacje sieci Web ogólnie obsługują obsługę plików statycznych, dynamiczną generację stron, internetowy interfejs API i najnowsze powiadomienia w czasie rzeczywistym. Należy oczekiwać, że każda z tych usług powinna być uruchamiana i zarządzana niezależnie.

Wymagało to pojedynczej abstrakcji hostingu, która umożliwia deweloperom tworzenie aplikacji na podstawie różnych składników i struktur, a następnie uruchamianie tej aplikacji na hoście pomocniczym.

## <a name="the-open-web-interface-for-net-owin"></a>Otwórz interfejs sieci Web dla platformy .NET (OWIN)

 Inspirowane korzyści płynących z używania [stojaka](http://rack.github.io/) w społeczności Ruby, kilku członków społeczności programu .NET Set out do tworzenia abstrakcji między serwerami sieci Web i składnikami struktury. Dwa cele projektowe dla abstrakcji OWIN były proste i że wymagały najmniejszej możliwej zależności od innych typów struktur. Te dwa cele pomagają zapewnić:

- Nowe składniki mogą być łatwiejsze do opracowania i zużyte.
- Aplikacje mogą być łatwiej przewoźny między hostami a potencjalnie całymi platformami/systemami operacyjnymi.

Wynikiem abstrakcyjnym składa się z dwóch podstawowych elementów. Pierwszy jest słownikiem środowiska. Ta struktura danych jest odpowiedzialna za przechowywanie wszystkich stanów niezbędnych do przetwarzania żądań i odpowiedzi HTTP, a także dowolnego odpowiedniego stanu serwera. Słownik środowiska jest definiowany w następujący sposób:

[!code-console[Main](an-overview-of-project-katana/samples/sample1.cmd)]

Serwer sieci Web zgodny z OWIN jest odpowiedzialny za wypełnienie słownika środowiska danymi, takimi jak strumienie treści i kolekcje nagłówków dla żądania HTTP i odpowiedzi. Następnie jest odpowiedzialny za składniki aplikacji lub struktury do wypełniania lub aktualizowania słownika z dodatkowymi wartościami i zapisu do strumienia treści odpowiedzi.

Oprócz określania typu słownika środowiska Specyfikacja OWIN definiuje listę par kluczowych wartości klucza słownika. Na przykład w poniższej tabeli przedstawiono wymagane klucze słownika dla żądania HTTP:

| Nazwa klucza | Opis wartości |
| --- | --- |
| `"owin.RequestBody"` | Strumień z treścią żądania (jeśli istnieje). W przypadku braku treści żądania wartość null może być używana jako symbol zastępczy. Zobacz [treść żądania](http://owin.org/html/owin.html#34-request-body-100-continue-and-completed-semantics). |
| `"owin.RequestHeaders"` | `IDictionary<string, string[]>` nagłówków żądania. Zobacz [nagłówki](http://owin.org/html/owin.html#3-3-headers). |
| `"owin.RequestMethod"` | `string` zawierający metodę żądania HTTP żądania (np., `"GET"`, `"POST"`). |
| `"owin.RequestPath"` | `string` zawierający ścieżkę żądania. Ścieżka musi być odnosząca się do elementu "root" delegata aplikacji; Zobacz [ścieżki](http://owin.org/html/owin.html#5-3-paths). |
| `"owin.RequestPathBase"` | `string` zawierający część ścieżki żądania odpowiadającej elementowi głównemu delegata aplikacji; Zobacz [ścieżki](http://owin.org/html/owin.html#5-3-paths). |
| `"owin.RequestProtocol"` | `string` zawierający nazwę i wersję protokołu (np. `"HTTP/1.0"` lub `"HTTP/1.1"`). |
| `"owin.RequestQueryString"` | `string` zawierający składnik ciągu zapytania identyfikatora URI żądania HTTP bez wiodącego "?" (np. `"foo=bar&baz=quux"`). Wartość może być pustym ciągiem. |
| `"owin.RequestScheme"` | `string` zawierający schemat identyfikatora URI używany dla żądania (np. `"http"`, `"https"`); Zobacz [schemat identyfikatora URI](http://owin.org/html/owin.html#5-1-uri-scheme). |

Drugi klucz elementu OWIN jest delegatem aplikacji. Jest to sygnatura funkcji, która służy jako podstawowy interfejs między wszystkimi składnikami w aplikacji OWIN. Definicja delegata aplikacji jest następująca:

`Func<IDictionary<string, object>, Task>;`

Delegat aplikacji jest po prostu implementacją typu delegata Func, gdzie funkcja akceptuje słownik środowiska jako dane wejściowe i zwraca zadanie. Ten projekt ma kilka skutków dla deweloperów:

- Istnieje bardzo mała liczba zależności typów wymaganych do zapisania składników OWIN. Znacznie zwiększa to dostępność OWIN dla deweloperów.
- Projekt asynchroniczny umożliwia abstrakcję wydajną w zakresie obsługi zasobów obliczeniowych, szczególnie w większej liczbie operacji we/wy.
- Ponieważ delegat aplikacji jest niepodzielną jednostką wykonywania i ponieważ słownik środowiska jest przenoszony jako parametr w delegatze, składniki OWIN można łatwo połączyć ze sobą w celu utworzenia złożonych potoków przetwarzania HTTP.

Z perspektywy implementacji OWIN jest specyfikacją ([http://owin.org/html/owin.html](http://owin.org/html/owin.html)). Jego celem nie jest kolejna platforma internetowa, ale raczej Specyfikacja sposobu, w jaki platformy sieci Web i serwery sieci Web współdziałają.

Jeśli zbadasz [Owin](http://owin.org/) lub [Katana](https://github.com/aspnet/AspNetKatana/wiki), możesz również zauważyć, że [pakiet NuGet Owin](http://nuget.org/packages/Owin) i Owin. dll. Ta biblioteka zawiera pojedynczy interfejs, [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs), który formalizes i skodyfikowa sekwencję uruchamiania opisaną w [sekcji 4](http://owin.org/html/owin.html#4-application-startup) specyfikacji Owin. Chociaż nie jest to wymagane w celu kompilowania serwerów OWIN, interfejs [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs) zapewnia konkretny punkt odniesienia i jest używany przez składniki projektu Katana.

## <a name="project-katana"></a>Katana projektu

Zarówno Specyfikacja [Owin](http://owin.org/html/owin.html) , jak i *Owin. dll* są wysiłkami należącymi do społeczności i społecznościowymi działaniami typu open source, projekt [Katana](https://github.com/aspnet/AspNetKatana/wiki) reprezentuje zestaw składników Owin, które w dalszym ciągu są tworzone i udostępniane przez firmę Microsoft. Te składniki obejmują oba składniki infrastruktury, takie jak hosty i serwery, a także składniki funkcjonalne, takie jak składniki uwierzytelniania i powiązania z platformami, takimi jak [sygnalizujący](../../../signalr/index.md) i [ASP.NET internetowy interfejs API](../../../web-api/overview/getting-started-with-aspnet-web-api/index.md). Projekt ma następujące trzy cele wysokiego poziomu: 

- **Przenośne** — składniki powinny być łatwo zastępowane dla nowych składników, gdy staną się dostępne. Obejmuje to wszystkie typy składników, od platformy do serwera i hosta. Implikacją tego celu jest to, że struktury innych firm mogą być bezproblemowo uruchamiane na serwerach firmy Microsoft, a platforma Microsoft Framework może być uruchamiana na serwerach i hostach innych firm.
- **Modularna i elastyczna**— w przeciwieństwie do wielu platform, które obejmują wyposażono funkcje, które są domyślnie włączone, Katana składniki projektu powinny być małe i skoncentrowane, zapewniając kontrolę nad deweloperem aplikacji w celu określenia składników do użycia w swojej aplikacji.
- **Lekkie/wydajne/skalowalne** — poprzez rozdzielenie tradycyjnego koncepcji struktury na zestaw małych i skoncentrowanych składników, które są dodawane jawnie przez dewelopera aplikacji, wynikowa aplikacja Katana może zużywać mniej zasobów obliczeniowych i w efekcie obsłużyć więcej obciążeń niż z innymi typami serwerów i struktur. Zgodnie z wymaganiami aplikacji dotyczącymi większej liczby funkcji dostępnych w ramach podstawowej infrastruktury, można je dodać do potoku OWIN, ale powinno to być wyraźną decyzją dla deweloperów aplikacji. Ponadto podstawianie składników niższego poziomu oznacza, że staną się dostępne, nowe serwery o wysokiej wydajności mogą być bezproblemowo wprowadzane w celu poprawy wydajności aplikacji OWIN bez przerywania tych aplikacji.

## <a name="getting-started-with-katana-components"></a>Wprowadzenie ze składnikami Katana

Po raz pierwszy został wprowadzony, jeden z aspektów środowiska [Node. js](http://nodejs.org/) , który bezpośrednio nastąpiło do uproszczenia, z którym może utworzyć i uruchomić serwer sieci Web. Jeśli cele Katana były w tym samym czasie zgodne z platformą [Node. js](http://nodejs.org/), jedna z nich może je podsumowywać, mówiąc, że Katana oferuje wiele zalet platformy [Node. js](http://nodejs.org/) (i struktur, takich jak IT) bez wymuszania, aby Deweloper nie wyrzucał wszystkich informacji o tworzeniu aplikacji sieci Web ASP.NET. Aby ta instrukcja była zastosowana do wartości true, wprowadzenie do projektu Katana powinno być równie proste w charakterze środowiska [Node. js](http://nodejs.org/).

## <a name="creating-hello-world"></a>Tworzenie "Hello world!"

Jedną z istotnych różnic między programowaniem JavaScript i .NET jest obecność (lub nieobecność) kompilatora. W związku z tym punktem wyjścia dla prostego serwera Katana jest projekt programu Visual Studio. Można jednak zacząć od najmniejszej liczby typów projektów: pustą aplikację sieci Web ASP.NET.

[![](an-overview-of-project-katana/_static/image1.png)](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb)

Następnie zainstalujemy pakiet NuGet [Microsoft. Owin. host. SystemWeb](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) w projekcie. Ten pakiet udostępnia serwer OWIN, który działa w potoku żądania ASP.NET. Można go znaleźć w [galerii NuGet](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) i można go zainstalować za pomocą okna dialogowego Menedżera pakietów programu Visual Studio lub konsoli Menedżera pakietów przy użyciu następującego polecenia:

[!code-console[Main](an-overview-of-project-katana/samples/sample2.cmd)]

Zainstalowanie pakietu `Microsoft.Owin.Host.SystemWeb` zainstaluje kilka dodatkowych pakietów jako zależności. Jedną z tych zależności jest `Microsoft.Owin`, biblioteki, która udostępnia kilka typów pomocników i metod tworzenia aplikacji OWIN. Możemy używać tych typów, aby szybko napisać następujący serwer "Hello World".

[!code-csharp[Main](an-overview-of-project-katana/samples/sample3.cs)]

Ten bardzo prosty serwer sieci Web można teraz uruchomić przy użyciu polecenia **F5** programu Visual Studio i zapewnia pełną obsługę debugowania.

## <a name="switching-hosts"></a>Przełączanie hostów

Domyślnie poprzedni przykład "Hello World" jest uruchamiany w potoku żądania ASP.NET, który używa System. Web w kontekście usług IIS. Pozwala to na dodanie ogromnej wartości, ponieważ pozwala nam wykorzystać elastyczność i możliwość tworzenia potoku OWIN z możliwościami zarządzania i ogólną zapadalnością usługi IIS. Mogą jednak wystąpić sytuacje, w których korzyści udostępniane przez usługi IIS nie są wymagane, a życzenie jest dla mniejszego, bardziej uproszczonego hosta. Co jest potrzebne, a następnie, aby uruchomić nasz prosty serwer sieci Web poza usługami IIS i system. Web?

Aby zilustrować cel przenośności, przechodzenie z hosta serwera sieci Web do hosta wiersza polecenia wymaga po prostu dodania nowych zależności serwera i hosta do folderu wyjściowego projektu, a następnie uruchomienie hosta. W tym przykładzie będziemy hostować nasz serwer sieci Web na hoście Katana o nazwie `OwinHost.exe` i będą używać serwera z systemem Katana odbiornika HttpListener. Podobnie jak w przypadku innych składników Katana, zostaną one uzyskane z NuGet przy użyciu następującego polecenia:

[!code-console[Main](an-overview-of-project-katana/samples/sample4.cmd)]

W wierszu polecenia można następnie przejść do folderu głównego projektu i po prostu uruchomić `OwinHost.exe` (który został zainstalowany w folderze Tools odpowiedniego pakietu NuGet). Domyślnie `OwinHost.exe` jest skonfigurowany do wyszukiwania serwera z systemem odbiornika HttpListener i dlatego nie jest wymagana żadna dodatkowa konfiguracja. Nawigowanie w przeglądarce internetowej w celu `http://localhost:5000/` pokazuje, że aplikacja działa teraz za pomocą konsoli programu.

![](an-overview-of-project-katana/_static/image2.png)

## <a name="katana-architecture"></a>Architektura Katana

 Architektura składnika Katana dzieli aplikację na cztery warstwy logiczne, jak przedstawiono poniżej: *host, serwer, oprogramowanie pośredniczące* i *aplikacja*. Architektura składników jest współczynnikiem w taki sposób, że implementacje tych warstw można łatwo zastępować w wielu przypadkach, bez konieczności ponownej kompilacji aplikacji.   

![](an-overview-of-project-katana/_static/image3.png)

## <a name="host"></a>Host

 Host jest odpowiedzialny za:

- Zarządzanie procesem podstawowym.
- Organizowanie przepływu pracy, który powoduje wybranie serwera i konstruowanie potoku OWIN, za pomocą którego będą obsługiwane żądania.

  Obecnie istnieją trzy podstawowe opcje hostingu dla aplikacji opartych na Katana:  
  
**IIS/ASP. NET**: korzystając ze standardowego modułu HttpModule i typów HttpHandler, potoki Owin można uruchamiać w usługach IIS jako część przepływu żądania ASP.NET. Obsługa hostingu ASP.NET jest włączana przez zainstalowanie pakietu NuGet Microsoft. AspNet. host. SystemWeb w projekcie aplikacji sieci Web. Ponadto, ponieważ usługi IIS pełnią rolę hosta i serwera, rozróżnianie serwera OWIN/hosta jest naliczane w tym pakiecie NuGet, co oznacza, że w przypadku korzystania z hosta SystemWeb, Deweloper nie może zastąpić alternatywnej implementacji serwera.  
  
**Host niestandardowy**: pakiet składników Katana zapewnia deweloperowi możliwość hostowania aplikacji we własnym procesie niestandardowym, niezależnie od tego, czy jest to Aplikacja konsolowa, usługa systemu Windows itd. Ta funkcja wygląda podobnie do możliwości samoobsługowego udostępniania przez internetowy interfejs API. Poniższy przykład przedstawia niestandardowy host kodu internetowego interfejsu API:  

[!code-csharp[Main](an-overview-of-project-katana/samples/sample5.cs)]

Konfiguracja samodzielnego hosta dla aplikacji Katana jest podobna:

[!code-csharp[Main](an-overview-of-project-katana/samples/sample6.cs)]

Jedną z istotnych różnic między interfejsem API sieci Web i Katana na własne przykłady jest brak kodu konfiguracji interfejsu API sieci Web w przykładowym przykładzie z własnym hostem Katana. Aby umożliwić przenośność i możliwość tworzenia, program Katana oddziela kod, który uruchamia serwer od kodu, który konfiguruje potok przetwarzania żądań. Kod, który konfiguruje internetowy interfejs API, jest zawarty w uruchamianiu klasy, który jest dodatkowo określony jako parametr typu w WebApplication. Start.

[!code-csharp[Main](an-overview-of-project-katana/samples/sample7.cs)]

Klasa startowa zostanie omówiona bardziej szczegółowo w dalszej części artykułu. Jednak kod wymagany do uruchomienia procesu samodzielnego hosta programu Katana wygląda podobnie do kodu, który może być używany dzisiaj w aplikacjach do samodzielnego hostowania interfejsu API sieci Web ASP.NET.

**OwinHost. exe**: podczas gdy niektóre z nich chcą napisać niestandardowy proces do uruchamiania aplikacji sieci Web Katana, wiele woli się po prostu uruchomić wstępnie utworzony plik wykonywalny, który może uruchomić serwer i uruchomić aplikację. W tym scenariuszu pakiet składników Katana zawiera `OwinHost.exe`. W przypadku uruchomienia z katalogu głównego projektu, ten plik wykonywalny uruchomi serwer (domyślnie używa serwera odbiornika HttpListener) i używa konwencji do znajdowania i uruchamiania klasy uruchamiania użytkownika. Aby uzyskać bardziej szczegółową kontrolę, plik wykonywalny zawiera szereg dodatkowych parametrów wiersza polecenia.

![](an-overview-of-project-katana/_static/image4.png)

## <a name="server"></a>Serwer

 Gdy host jest odpowiedzialny za uruchamianie i konserwowanie procesu, w którym działa aplikacja, odpowiedzialność za serwer jest otwarcie gniazda sieciowego, nasłuchiwanie żądań i wysłanie ich za pomocą potoku składników OWIN określonych przez użytkownika (podczas mógł już zauważono, ten potok jest określony w klasie startowej dewelopera aplikacji). Obecnie projekt Katana obejmuje dwie implementacje serwera: 

- **Microsoft. Owin. host. SystemWeb**: jak wspomniano wcześniej, usługi IIS w połączeniu z potokiem ASP.NET pełnią rolę hosta i serwera. W związku z tym w przypadku wybrania tej opcji hostingu usługi IIS są zarządzane na poziomie hosta, takie jak Aktywacja procesu i nasłuchuje żądań HTTP. W przypadku aplikacji sieci Web ASP.NET wysyła żądania do potoku ASP.NET. Host Katana SystemWeb rejestruje ASP.NET HttpModule i HttpHandler, aby przechwycić żądania w miarę ich przepływu przez potok HTTP i wysyłać je za pośrednictwem potoku przez użytkownika OWIN.
- **Microsoft. Owin. host. odbiornika HttpListener**: jako nazwa wskazuje, że ten serwer Katana używa klasy odbiornika HttpListener .NET Framework, aby otworzyć gniazdo i wysyłać żądania do potoku programu Owin określonego przez dewelopera. Jest to obecnie domyślny wybór serwera dla interfejsu API samoobsługowego Katana i OwinHost. exe.

## <a name="middlewareframework"></a>Oprogramowanie pośredniczące/platforma

 Jak wspomniano wcześniej, gdy serwer zaakceptuje żądanie od klienta, jest odpowiedzialny za przekazanie go za pośrednictwem potoku składników OWIN, które są określone przez kod startowy dewelopera. Te składniki potoku są znane jako oprogramowanie pośredniczące.  
 Na bardzo podstawowym poziomie składnik oprogramowania pośredniczącego OWIN po prostu musi wdrożyć delegata aplikacji OWIN, aby możliwe było jego wywoływanie.

[!code-console[Main](an-overview-of-project-katana/samples/sample8.cmd)]

Jednak w celu uproszczenia programowania i kompozycji komponentów oprogramowania pośredniczącego Katana obsługuje kilku Konwencji i typów pomocnika dla składników oprogramowania pośredniczącego. Najczęstszym z nich jest Klasa `OwinMiddleware`. Niestandardowy składnik pośredniczący utworzony przy użyciu tej klasy będzie wyglądać podobnie do poniższego: 

[!code-csharp[Main](an-overview-of-project-katana/samples/sample9.cs)]

 Ta klasa pochodzi z `OwinMiddleware`, implementuje konstruktora, który akceptuje wystąpienie następnego oprogramowania pośredniczącego w potoku jako jeden z jego argumentów, a następnie przekazuje go do konstruktora podstawowego. Dodatkowe argumenty używane do konfigurowania oprogramowania pośredniczącego są również zadeklarowane jako parametry konstruktora po następnym parametrze pośredniczącego oprogramowania.   
  
W czasie wykonywania oprogramowanie pośredniczące jest wykonywane za pośrednictwem zastąpionej metody `Invoke`. Ta metoda przyjmuje pojedynczy argument typu `OwinContext`. Ten obiekt kontekstu jest dostarczany przez pakiet NuGet `Microsoft.Owin` opisany wcześniej i zapewnia dostęp silnie określony do żądania, odpowiedzi i słownika środowiska, a także kilka dodatkowych typów pomocnika.   
  
Klasy pośredniczącej można łatwo dodać do potoku OWIN w kodzie uruchamiania aplikacji w następujący sposób:   

[!code-csharp[Main](an-overview-of-project-katana/samples/sample10.cs)]

Ponieważ infrastruktura Katana po prostu kompiluje potok składników oprogramowania pośredniczącego OWIN, ponieważ składniki muszą po prostu obsługiwać delegata aplikacji, aby uczestniczył w potoku, składniki pośredniczące mogą obejmować złożoność od prostych rejestratorów do całych platform, takich jak ASP.NET, Web API lub [sygnalizujący](../../../signalr/index.md). Na przykład dodanie interfejsu API sieci Web ASP.NET do poprzedniego potoku OWIN wymaga dodania następującego kodu uruchamiania:

[!code-csharp[Main](an-overview-of-project-katana/samples/sample11.cs)]

Infrastruktura Katana będzie kompilować potok komponentów oprogramowania pośredniczącego na podstawie kolejności, w której zostały dodane do obiektu IAppBuilder w metodzie konfiguracji. W naszym przykładzie LoggerMiddleware może obsłużyć wszystkie żądania, które przepływają przez potok, niezależnie od tego, jak te żądania są ostatecznie obsługiwane. Pozwala to na zaawansowane scenariusze, w których składnik pośredniczący (np. składnik uwierzytelniania) może przetwarzać żądania dla potoku obejmującego wiele składników i struktur (np. ASP.NET Web API, Sygnalizującer i statyczny serwer plików).
 
## <a name="applications"></a>Aplikacje

Jak pokazano w poprzednich przykładach, OWIN i projekt Katana nie powinien być uważany za nowy model programowania aplikacji, ale raczej jako abstrakcję, aby oddzielić modele programowania aplikacji i struktury z serwerów i infrastruktury hostingu. Na przykład podczas kompilowania aplikacji interfejsu Web API platforma dewelopera będzie nadal korzystać z struktury internetowego interfejsu API ASP.NET, niezależnie od tego, czy aplikacja działa w potoku OWIN przy użyciu składników z projektu Katana. To miejsce, gdzie kod powiązany OWIN będzie widoczny dla dewelopera aplikacji, będzie kodem uruchomienia aplikacji, w którym deweloper składa potok OWIN. W kodzie uruchomienia deweloper będzie rejestrował serię UseXx instrukcji, ogólnie jeden dla każdego składnika pośredniczącego, który będzie przetwarzać żądania przychodzące. To środowisko będzie miało taki sam skutek jak rejestrowanie modułów HTTP w bieżącym świecie system. Web. Zwykle większe oprogramowanie pośredniczące platformy, takie jak ASP.NET Web API lub [sygnalizujący](../../../signalr/index.md) , zostanie zarejestrowane na końcu potoku. Wycinanie oprogramowania pośredniczącego, takiego jak te na potrzeby uwierzytelniania lub buforowania, jest zwykle rejestrowane na początku potoku, aby przetwarzać żądania dla wszystkich platform i składników zarejestrowanych w dalszej części potoku. Takie rozdzielenie części oprogramowania pośredniczącego od siebie i z podstawowych składników infrastruktury pozwala składnikom rozwijać się w różnych szybkościach, a jednocześnie zapewnić, że cały system pozostaje stabilny.

## <a name="components--nuget-packages"></a>Składniki — pakiety NuGet

Podobnie jak w przypadku wielu bieżących bibliotek i struktur, składniki projektu Katana są dostarczane jako zestaw pakietów NuGet. W przypadku nadchodzącej wersji 2,0 Graf zależności pakietu Katana wygląda następująco. (Kliknij obraz, aby wyświetlić większy widok).

[![](an-overview-of-project-katana/_static/image6.png)](an-overview-of-project-katana/_static/image5.png)

Niemal każdy pakiet w projekcie Katana zależy, bezpośrednio lub pośrednio, w pakiecie Owin. Można pamiętać, że jest to pakiet, który zawiera interfejs IAppBuilder, który zapewnia konkretną implementację sekwencji uruchamiania aplikacji opisaną w sekcji 4 specyfikacji OWIN. Ponadto wiele pakietów jest zależne od Microsoft. Owin, który dostarcza zestaw typów pomocników do pracy z żądaniami HTTP i odpowiedziami. Pozostała część pakietu może być sklasyfikowana jako hostowanie pakietów infrastruktury (serwerów lub hostów) lub oprogramowania pośredniczącego. Pakiety i zależności, które są zewnętrzne w projekcie Katana, są wyświetlane w kolorze pomarańczowym.

Infrastruktura hostingu dla Katana 2,0 obejmuje zarówno serwery SystemWeb, jak i odbiornika HttpListener, pakiet OwinHost do uruchamiania aplikacji OWIN przy użyciu programu OwinHost. exe oraz pakiet Microsoft. Owin. hosting do samodzielnego hostowania aplikacji OWIN w Host niestandardowy (np. Aplikacja konsolowa, usługa systemu Windows itp.)

W przypadku Katana 2,0 składniki pośredniczące głównie koncentrują się na udostępnianiu różnych metod uwierzytelniania. Dostępny jest jeden dodatkowy składnik pośredniczący do celów diagnostycznych, który umożliwia obsługę strony początkowej i błędu. W miarę jak OWIN zwiększa abstrakcję hostingu, ekosystem części oprogramowania pośredniczącego, zarówno tych opracowanych przez firmę Microsoft, jak i innych firm, również zostanie powiększony do liczby.

## <a name="conclusion"></a>Podsumowanie

 Od początku nie ma potrzeby tworzenia i wymuszania, aby deweloperzy mogli już poznać inne platformy sieci Web. Zamiast tego celem jest utworzenie streszczenia, aby umożliwić deweloperom aplikacji sieci Web .NET większy wybór niż wcześniej. Dzięki rozdzieleniu warstwy logicznej typowego stosu aplikacji sieci Web na zestaw składników wymiennych, projekt Katana zapewnia składniki w całym stosie, aby zwiększyć ich szybkość. Przez skompilowanie wszystkich składników wokół prostej abstrakcji OWIN, Katana umożliwia obsługę struktur i aplikacji utworzonych na ich podstawie do przenoszenia między różnymi serwerami i hostami. Przez umieszczenie dewelopera w kontroli nad stosem, Katana gwarantuje, że programista podejmuje ostateczny wybór o tym, jak uproszczony lub w jaki sposób bogate w funkcje stos sieci Web.  

## <a name="for-more-information-about-katana"></a>Aby uzyskać więcej informacji na temat Katana

- Projekt Katana w witrynie GitHub: [https://github.com/aspnet/AspNetKatana/](https://github.com/aspnet/AspNetKatana/).
- Wideo: [Katana Project-Owin dla ASP.NET](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET), przez Howard Dierking.

## <a name="acknowledgements"></a>Podziękowania

- [Rick Anderson](https://blogs.msdn.com/b/rickandy/): (Twitter [@RickAndMSFT](http://twitter.com/RickAndMSFT) ) Rick to starszy moduł zapisujący Programowanie dla firmy Microsoft skupiając się na platformie Azure i MVC.
- [Scott Hanselman](http://www.hanselman.com/blog/): (Twitter [@shanselman](https://twitter.com/shanselman) )
- [Jan Galloway](https://weblogs.asp.net/jgalloway/default.aspx): (Twitter [@jongalloway](https://twitter.com/jongalloway) )

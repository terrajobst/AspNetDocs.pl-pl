---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: Co nie rób w programie ASP.NET i co należy zrobić w zamian | Microsoft Docs
author: Rick-Anderson
description: W tym temacie opisano kilka typowych błędów podejmowanych przez osoby w ramach projektów sieci Web ASP.NET. Zawiera zalecenia dotyczące tego, co należy zrobić, aby uniknąć tych Commo...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 980d3544df70643043391e6573803ce21b3a824f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616992"
---
# <a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a>Czego nie robić na platformie ASP.NET i co zrobić zamiast tego

> W tym temacie opisano kilka typowych błędów podejmowanych przez osoby w ramach projektów sieci Web ASP.NET. Zawiera zalecenia dotyczące tego, co należy zrobić, aby uniknąć tych typowych błędów. Jest on oparty na [prezentacji](http://vimeo.com/68390507) **Damian Edwards** na konferencji dla deweloperów w języku norweskim.

## <a name="disclaimer"></a>Zastrzeżenie

Ten temat nie jest przeznaczony dla pełnego przewodnika, aby zapewnić bezpieczeństwo i wydajność aplikacji. Nadal należy stosować najlepsze rozwiązania w zakresie zabezpieczeń i wydajności, które nie zostały opisane w tym temacie. Sugeruje tylko, jak uniknąć typowych błędów związanych z klasami i procesami .NET.

## <a name="overview"></a>Omówienie

Ten temat zawiera następujące sekcje:

- [Zgodność ze standardami](#standards)

    - [Karty sterowania](#adapters)
    - [Właściwości stylu w kontrolkach](#styleprop)
    - [Wywołania zwrotne strony i sterowania](#callback)
    - [Wykrywanie możliwości przeglądarki](#browsercap)
- [Bezpieczeństwo](#security)

    - [Żądaj weryfikacji](#validation)
    - [Uwierzytelnianie i sesja formularzy bez plików cookie](#cookieless)
    - [EnableViewStateMac](#viewstatemac)
    - [Średnie zaufanie](#medium)
    - [&lt;appSettings&gt;](#appsettings)
    - [UrlPathEncode](#urlpathencode)
- [Niezawodność i wydajność](#performance)

    - [PreSendRequestHeaders i PreSendRequestContent](#presend)
    - [Asynchroniczne zdarzenia strony z formularzami sieci Web](#asyncevents)
    - [Działanie ognia i zapomnij](#fire)
    - [Treść jednostki żądania](#requestentity)
    - [Response. Redirect i Response. end](#redirect)
    - [EnableViewState i ViewStatemode](#viewstatemode)
    - [SqlMembershipProvider](#sqlprovider)
    - [Długotrwałe żądania (> 110 s)](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a>Zgodność ze standardami

<a id="adapters"></a>

### <a name="control-adapters"></a>Karty sterowania

Zalecenie: Przestań korzystać z adapterów kontrolnych na potrzeby adaptacyjnego renderowania, a zamiast tego użyj zapytań dotyczących multimediów CSS i HTML zgodnych ze standardami.

Karty sterowania zostały wprowadzone w programie .NET 2,0 w celu renderowania kodu prezentacji, który został dostosowany dla różnych urządzeń i środowisk. Teraz takie adaptacyjne renderowanie można wykonać za pomocą CSS i HTML. Należy zatrzymać używanie adapterów sterowania i przekonwertować wszystkie istniejące karty na CSS i HTML.

Aby uzyskać więcej informacji, zobacz temat [zapytania dotyczące multimediów](http://www.w3.org/TR/css3-mediaqueries/) i [instrukcje: dodawanie stron mobilnych do aplikacji Web Forms/MVC ASP.NET](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a>Właściwości stylu w kontrolkach

Zalecenie: Zatrzymaj Ustawianie wartości stylu w znaczniku kontrolki, a zamiast tego Ustaw wartości formatowania w arkuszach stylów CSS.

Formanty serwera sieci Web zawierają dziesiątki właściwości, których można użyć do ustawiania właściwości stylu wbudowanego. Na przykład właściwość ForeColor ustawia kolor tekstu dla kontrolki. Ten sam efekt można osiągnąć wydajniej za poorednictwem arkuszy stylów CSS. Arkusze stylów umożliwiają scentralizowanie wartości stylu i unikanie ustawiania tych wartości w całej aplikacji.

Poniższy przykład pokazuje klasę CSS, ustawia tekst na czerwony.

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

W następnym przykładzie pokazano sposób dynamicznego zastosowania klasy CSS.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a>Wywołania zwrotne strony i sterowania

Zalecenie: Przestań używać funkcji wywołania zwrotne stron i kontrolek, a zamiast tego użyj dowolnej z następujących metod: AJAX, UpdatePanel, MVC, WebAPI lub Signal.

We wcześniejszych wersjach ASP.NET metody wywołania zwrotnego strony i kontroli umożliwiają aktualizowanie części strony sieci Web bez odświeżania całej strony. Teraz można wykonywać aktualizacje częściowej strony za poorednictwem [technologii AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [Web API](../../../web-api/index.md) lub [sygnalizującego](../../../signalr/index.md). Należy zatrzymać korzystanie z metod wywołania zwrotnego, ponieważ mogą one powodować problemy z przyjaznymi adresami URL i routingiem. Domyślnie formanty nie włączają metod wywołania zwrotnego, ale jeśli ta funkcja została włączona w kontrolce, należy ją wyłączyć.

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a>Wykrywanie możliwości przeglądarki

Zalecenie: Przestań korzystać z wykrywania możliwości przeglądarki statycznej, a zamiast tego użyj wykrywania funkcji dynamicznych.

We wcześniejszych wersjach programu ASP.NET obsługiwane funkcje dla każdej przeglądarki zostały zapisane w pliku XML. Wykrywanie obsługi funkcji za pomocą wyszukiwania statycznego nie jest najlepszym rozwiązaniem. Teraz można dynamicznie wykryć obsługiwane funkcje przeglądarki za pomocą platformy wykrywania funkcji, takiej jak program [modernizacja](http://modernizr.com/). Wykrywanie funkcji określa pomoc techniczną, próbując użyć metody lub właściwości, a następnie sprawdzić, czy przeglądarka wygenerowała żądany wynik. Domyślnie program modernizowany jest dołączony do szablonów aplikacji sieci Web.

<a id="security"></a>

## <a name="security"></a>Zabezpieczenia

<a id="validation"></a>

### <a name="request-validation"></a>Żądaj weryfikacji

Zalecenie: Weryfikuj dane wejściowe użytkownika i Koduj dane wyjściowe od użytkowników.

Sprawdzanie poprawności żądania jest funkcją ASP.NET, która sprawdza każde żądanie i przerywa żądanie, jeśli zostanie znalezione postrzegane zagrożenie. Nie należy polegać na weryfikacji żądań w celu zabezpieczania aplikacji przed atakami na skrypty między lokacjami. Zamiast tego Sprawdź poprawność wszystkich danych wejściowych od użytkowników i Koduj dane wyjściowe. W niektórych ograniczonych przypadkach można użyć wyrażeń regularnych do walidacji danych wejściowych, ale w bardziej skomplikowanych przypadkach należy zweryfikować dane wejściowe użytkownika przy użyciu klas platformy .NET, które określają, czy wartość jest zgodna z dozwolonymi wartościami.

Poniższy przykład pokazuje, jak używać metody statycznej w klasie URI w celu ustalenia, czy identyfikator URI podany przez użytkownika jest prawidłowy.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

Aby jednak zapewnić wystarczającą weryfikację identyfikatora URI, należy również sprawdzić, czy określa `http` lub `https`. W poniższym przykładzie są używane metody wystąpień do sprawdzenia, czy identyfikator URI jest prawidłowy.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

Przed renderowaniem danych wejściowych użytkownika jako HTML lub z uwzględnieniem danych wejściowych użytkownika w kwerendzie SQL Zakoduj wartości w celu zapewnienia, że złośliwy kod nie jest uwzględniony.

Możesz kodować wartość w kodzie HTML za pomocą składni &lt;%:%&gt;, jak pokazano poniżej.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

Lub, w składnia Razor, można kodować kod HTML za pomocą @, jak pokazano poniżej.

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

W następnym przykładzie pokazano, jak kod HTML kodować wartość w kodzie.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

Aby bezpiecznie zakodować wartość poleceń SQL, należy użyć parametrów polecenia, takich jak [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx). <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a>Uwierzytelnianie i sesja formularzy bez plików cookie

Zalecenie: Wymagaj plików cookie.

Przekazywanie informacji o uwierzytelnianiu w ciągu zapytania nie jest bezpieczne. W związku z tym Wymagaj plików cookie, gdy aplikacja obejmuje uwierzytelnianie. Jeśli plik cookie przechowuje poufne informacje, należy rozważyć wymaganie protokołu SSL dla pliku cookie.

Poniższy przykład pokazuje, jak określić w pliku Web. config, który uwierzytelnianie formularzy wymaga pliku cookie, który jest przesyłany za pośrednictwem protokołu SSL.

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a>EnableViewStateMac

Zalecenie: nigdy nie ustawiono wartości false.

Domyślnie EnableViewStateMac ma wartość true. Nawet jeśli aplikacja nie używa stanu widoku, nie należy ustawiać EnableViewStateMac na false. Ustawienie tej wartości na false spowoduje, że aplikacja będzie narażona na obsługę skryptów między lokacjami.

Począwszy od ASP.NET 4.5.2, środowisko uruchomieniowe wymusza **EnableViewStateMac = true**. Nawet jeśli zostanie ustawiona na wartość false, środowisko uruchomieniowe zignoruje tę wartość i kontynuuje wartość ustawioną na wartość true. Aby uzyskać więcej informacji, zobacz [ASP.NET 4.5.2 i EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).

Poniższy przykład pokazuje, jak ustawić EnableViewStateMac na true. Nie ma potrzeby rzeczywistego ustawiania tej wartości na wartość true, ponieważ jest ona domyślnie prawdziwa. Jeśli jednak ustawisz wartość false na dowolnej stronie aplikacji, musisz natychmiast poprawić tę wartość.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a>Średnie zaufanie

Zalecenie: nie należy polegać na średnim zaufaniu (lub żadnym innym poziomie zaufania) jako granicy zabezpieczeń.

Częściowe zaufanie nie chroni odpowiedniej aplikacji i nie powinno być używane. Zamiast tego należy używać pełnego zaufania i izolować aplikacje niezaufane w oddzielnych pulach aplikacji. Ponadto Uruchom każdą pulę aplikacji w ramach unikatowej tożsamości. Aby uzyskać więcej informacji, zobacz [ASP.NET częściowe zaufanie nie gwarantuje izolacji aplikacji](https://support.microsoft.com/kb/2698981).

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a>&lt;appSettings&gt;

Zalecenie: nie należy wyłączać ustawień zabezpieczeń w &lt;appSettings&gt; elementu.

Element appSettings zawiera wiele wartości, które są wymagane dla aktualizacji zabezpieczeń. Nie należy zmieniać ani wyłączać tych wartości. Jeśli podczas wdrażania aktualizacji należy wyłączyć te wartości, po zakończeniu wdrażania ponownie włącz je.

Aby uzyskać szczegółowe informacje, zobacz [ASP.NET AppSettings elementu](https://msdn.microsoft.com/library/hh975440.aspx).

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a>UrlPathEncode

Zalecenie: zamiast tego należy użyć [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) .

Metoda UrlPathEncode została dodana do .NET Framework, aby rozwiązać szczególny problem ze zgodnością przeglądarki. Nie jest to odpowiednio zakodowany adres URL i nie chroni aplikacji przed wykonywaniem skryptów między lokacjami. Nigdy nie należy używać go w aplikacji. Zamiast tego należy użyć [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).

Poniższy przykład pokazuje, jak przekazać zakodowany adres URL jako parametr ciągu zapytania dla kontrolki Hyperlink.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a>Wydajność i niezawodność

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a>PreSendRequestHeaders i PreSendRequestContent

Zalecenie: nie używaj tych zdarzeń z modułami zarządzanymi. Zamiast tego należy napisać natywny moduł usług IIS w celu wykonania wymaganego zadania. Zobacz [Tworzenie modułów HTTP kodu natywnego](https://msdn.microsoft.com/library/ms693629.aspx).

Można użyć zdarzeń [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) i [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) z NATYWNYMI modułami usług IIS.
> [!WARNING]
> Nie należy używać `PreSendRequestHeaders` i `PreSendRequestContent` z modułami zarządzanymi, które implementują `IHttpModule`. Ustawienie tych właściwości może powodować problemy z żądaniami asynchronicznymi. Kombinacja aplikacji do routingu (ARR) i obiektów WebSockets może prowadzić do wyjątków naruszeń, które mogą spowodować awarię w3wp. Na przykład iiscore! W3_CONTEXT_BASE:: GetIsLastNotification + 68 w iiscore. dll spowodował wyjątek naruszenia zasad dostępu (0xC0000005).

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a>Asynchroniczne zdarzenia strony z formularzami sieci Web

Zalecenie: w formularzach sieci Web Unikaj pisania asynchronicznych metod dla zdarzeń związanych z cyklem życia strony, a zamiast tego użyj [Page. RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) dla kodu asynchronicznego.

Po oznaczeniu zdarzenia strony z opcją **Async** i **void**nie można określić, kiedy kod asynchroniczny został zakończony. Zamiast tego należy użyć Page. RegisterAsyncTask, aby uruchomić kod asynchroniczny w sposób umożliwiający śledzenie jego ukończenia.

Poniższy przykład pokazuje przycisk obsługi, który zawiera kod asynchroniczny. Ten przykład obejmuje Odczytywanie wartości typu ciąg asynchronicznie, który jest dostarczany tylko jako uproszczony przykład zadania asynchronicznego, a nie jako zalecane rozwiązanie.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

W przypadku korzystania z zadań asynchronicznych należy ustawić platformę docelową http środowiska uruchomieniowego na 4,5 (lub nowszą) w pliku Web. config. Ustawienie platformy docelowej na 4,5 powoduje włączenie nowego kontekstu synchronizacji, który został dodany w programie .NET 4,5. Ta wartość jest domyślnie ustawiana w nowych projektach w programie Visual Studio, ale nie jest ustawiona, jeśli pracujesz z istniejącym projektem.

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a>Działanie ognia i zapomnij

Zalecenie: podczas obsługi żądania w ramach ASP.NET, unikaj uruchamiania i zapomnienia działania (takie jak wywołanie metody puli wątków. QueueUserWorkItem lub utworzenie czasomierza, który wielokrotnie wywołuje delegata).

Jeśli aplikacja ma działające w ramach programu ASP.NET i zapomnij, że aplikacja może nie być zsynchronizowana. W dowolnym momencie można zniszczyć domenę aplikacji, co oznacza, że ciągły proces nie jest już zgodny z bieżącym stanem aplikacji.

Ten typ pracy należy przenieść poza ASP.NET. Możesz użyć zadań sieci Web, usługi systemu Windows lub roli procesu roboczego na platformie Azure do wykonywania bieżących zadań i uruchamiania tego kodu z innego procesu.

Jeśli musisz wykonać tę operację w ASP.NET, możesz dodać pakiet NuGet o nazwie [Webbackgrounder](http://www.nuget.org/packages/webbackgrounder) , aby uruchomić kod.

<a id="requestentity"></a>

### <a name="request-entity-body"></a>Treść jednostki żądania

Zalecenie: unikanie odczytywania żądania. form lub Request. InputStream przed zdarzeniem wykonania procedury obsługi.

Najwcześniejsza powinna zostać odczytana z metody Request. form lub Request. InputStream w trakcie zdarzenia wykonania procedury obsługi. W MVC kontroler jest programem obsługi, a zdarzenie Execute jest wykonywane po uruchomieniu metody akcji. W formularzach sieci Web strona jest programem obsługi, a zdarzenie Execute jest wyzwalane po wypełnieniu zdarzenia Page. init. Jeśli odczytasz treść jednostki żądania wcześniejszą od zdarzenia Execute, zakłócasz przetwarzanie żądania.

Jeśli musisz odczytać treść jednostki żądania przed zdarzeniem Execute, użyj metody [Request. GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) lub [Request. GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx). Korzystając z GetBufferlessInputStream, uzyskujesz strumień nieprzetworzony z żądania i przyjmujemy odpowiedzialność za przetwarzanie całego żądania. Po wywołaniu metody GetBufferlessInputStream, Request. form i Request. InputStream są niedostępne, ponieważ nie zostały wypełnione przez ASP.NET. Gdy używasz GetBufferedInputStream, otrzymujesz kopię strumienia z żądania. Żądania. form i Request. InputStream są nadal dostępne w dalszej części żądania, ponieważ ASP.NET wypełnia drugą kopię.

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a>Response. Redirect i Response. end

Zalecenie: należy pamiętać o różnicach w sposobie obsługi wątku po wywołaniu metody [Response. Redirect (String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).

Metoda [Response. Redirect (String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) wywołuje metodę Response. end. W procesie synchronicznym wywołanie metody Request. Redirect powoduje natychmiastowe przerwanie działania bieżącego wątku. Jednak w procesie asynchronicznym wywołanie metody Response. Redirect nie przerywa bieżącego wątku, więc wykonanie kodu będzie kontynuowane dla żądania. W procesie asynchronicznym należy zwrócić zadanie z metody, aby zatrzymać wykonywanie kodu.

W projekcie MVC nie należy wywoływać metody Response. Redirect. Zamiast tego należy zwrócić RedirectResult.

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a>EnableViewState i ViewStatemode

Zalecenie: Użyj elementu ViewStatemode zamiast EnableViewState, aby zapewnić szczegółową kontrolę nad tym, które kontrolki używają stanu widoku.

Gdy ustawisz EnableViewState na false w dyrektywie page, stan widoku jest wyłączony dla wszystkich kontrolek na stronie i nie można go włączyć. Jeśli chcesz włączyć opcję wyświetlania stanu tylko dla niektórych kontrolek na stronie, ustaw wartość ViewStatemode na wyłączone dla strony.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

Następnie ustaw wartość ViewStatemode na włączone tylko dla kontrolek, które rzeczywiście wymagają stanu widoku.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

Przez włączenie stanu widoku tylko dla kontrolek, które go potrzebują, można zmniejszyć rozmiar widoku dla stron sieci Web.

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a>SqlMembershipProvider

Zalecenie: Użyj dostawców uniwersalnych.

W bieżących szablonach projektu SqlMembershipProvider został zastąpiony przez [dostawców uniwersalnych ASP.NET](http://www.nuget.org/packages/Microsoft.AspNet.Providers), który jest dostępny jako pakiet NuGet. Jeśli używasz SqlMembershipProvider w projekcie, który został skompilowany przy użyciu wcześniejszej wersji szablonów, należy przełączyć się na uniwersalne dostawcy. Dostawcy Uniwersalni pracują ze wszystkimi bazami danych, które są obsługiwane przez Entity Framework.

Aby uzyskać więcej informacji, zobacz [wprowadzenie do uniwersalnych dostawców ASP.NET](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a>Długotrwałe żądania (> 110 sekund)

Zalecenie: Użyj obiektów [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) lub [sygnalizujących](../../../signalr/index.md) dla podłączonych klientów i Użyj asynchronicznych operacji we/wy.

Długotrwałe żądania mogą spowodować nieprzewidywalne wyniki oraz niską wydajność aplikacji sieci Web. Domyślne ustawienie limitu czasu dla żądania wynosi 110 sekund. Jeśli używasz stanu sesji z długotrwałym żądaniem, ASP.NET zwolni blokadę obiektu sesji po 110 sekundach. Aplikacja może jednak znajdować się w środku operacji w obiekcie sesji, gdy blokada jest wydana, a operacja może nie zakończyć się pomyślnie. Jeśli drugie żądanie od użytkownika jest blokowane podczas pierwszego żądania, drugie żądanie może uzyskać dostęp do obiektu sesji w stanie niespójnym.

Jeśli aplikacja zawiera blokowanie (lub synchroniczne) operacje we/wy, aplikacja nie będzie odpowiadać.

Aby zwiększyć wydajność, Użyj asynchronicznych operacji we/wy w .NET Framework. Należy również użyć obiektów WebSockets lub sygnalizujących do łączenia klientów z serwerem. Te funkcje zostały zaprojektowane w celu wydajnego obsłużenia długotrwałych żądań.

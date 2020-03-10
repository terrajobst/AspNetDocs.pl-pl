---
uid: web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
title: Konfiguracja i Instrumentacja | Microsoft Docs
author: microsoft
description: Istnieją istotne zmiany w konfiguracji i Instrumentacji w programie ASP.NET 2,0. Nowy interfejs API konfiguracji ASP.NET umożliwia zmianę konfiguracji żądania ściągnięcia...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 21ebbaee-7ed8-45ae-b6c1-c27c88342e48
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
msc.type: authoredcontent
ms.openlocfilehash: cd5bedce5459e8cf8e72df8de69ebd82f2d97789
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78626029"
---
# <a name="configuration-and-instrumentation"></a>Konfiguracja i instrumentacja

przez [firmę Microsoft](https://github.com/microsoft)

> Istnieją istotne zmiany w konfiguracji i Instrumentacji w programie ASP.NET 2,0. Nowy interfejs API konfiguracji ASP.NET umożliwia programistyczne wprowadzanie zmian w konfiguracji. Ponadto istnieją liczne nowe ustawienia konfiguracji, które pozwalają na nowe konfiguracje i instrumentację.

Istnieją istotne zmiany w konfiguracji i Instrumentacji w programie ASP.NET 2,0. Nowy interfejs API konfiguracji ASP.NET umożliwia programistyczne wprowadzanie zmian w konfiguracji. Ponadto istnieją liczne nowe ustawienia konfiguracji, które pozwalają na nowe konfiguracje i instrumentację.

W tym module będziemy omawiać ASP.NET interfejs API konfiguracji jako odnoszący się do odczytu i zapisu do plików konfiguracji ASP.NET, a także obejmować instrumentację ASP.NET. Będziemy również pokryć nowe funkcje dostępne w śledzeniu ASP.NET.

## <a name="aspnet-configuration-api"></a>Interfejs API konfiguracji ASP.NET

Interfejs API konfiguracji ASP.NET umożliwia tworzenie i wdrażanie danych konfiguracji aplikacji oraz zarządzanie nimi przy użyciu jednego interfejsu programowania. Za pomocą interfejsu API konfiguracji można programistycznie i modyfikować kompletne konfiguracje ASP.NET bez bezpośredniego edytowania kodu XML w plikach konfiguracji. Ponadto można użyć interfejsu API konfiguracji w aplikacjach konsolowych i skryptów, które tworzysz, w narzędziach do zarządzania opartego na sieci Web oraz w przystawkach programu Microsoft Management Console (MMC).

Poniższe dwa narzędzia do zarządzania konfiguracją używają interfejsu API konfiguracji i są dołączone do .NET Framework wersji 2,0:

- Przystawka MMC ASP.NET, która używa interfejsu API konfiguracji do uproszczenia zadań administracyjnych, zapewniając Zintegrowany widok lokalnych danych konfiguracyjnych ze wszystkich poziomów hierarchii konfiguracji.
- Narzędzie do administrowania witryną sieci Web, które umożliwia zarządzanie ustawieniami konfiguracji dla aplikacji lokalnych i zdalnych, w tym hostowanych witryn.

Interfejs API konfiguracji ASP.NET składa się z zestawu obiektów zarządzania ASP.NET, których można użyć do programistycznego konfigurowania witryn i aplikacji sieci Web. Obiekty zarządzania są implementowane jako Biblioteka klas .NET Framework. Model programowania interfejsu API konfiguracji pomaga zapewnić spójność kodu i niezawodność, wymuszając typy danych w czasie kompilacji. Aby ułatwić zarządzanie konfiguracjami aplikacji, interfejs API konfiguracji umożliwia wyświetlanie danych scalonych ze wszystkich punktów w hierarchii konfiguracji jako pojedynczej kolekcji, zamiast wyświetlać dane jako oddzielne kolekcje z różnych pliki konfiguracji. Ponadto interfejs API konfiguracji umożliwia manipulowanie całymi konfiguracjami aplikacji bez bezpośredniego edytowania kodu XML w plikach konfiguracji. Na koniec interfejs API upraszcza zadania konfiguracji przez obsługę narzędzi administracyjnych, takich jak narzędzie do administrowania witryną sieci Web. Interfejs API konfiguracji upraszcza wdrażanie dzięki obsłudze tworzenia plików konfiguracji na komputerze i uruchamianiu skryptów konfiguracyjnych na wielu komputerach.

> [!NOTE]
> Interfejs API konfiguracji nie obsługuje tworzenia aplikacji usług IIS.

## <a name="working-with-local-and-remote-configuration-settings"></a>Praca z ustawieniami konfiguracji lokalnego i zdalnego

Obiekt konfiguracji reprezentuje scalony widok ustawień konfiguracji, które mają zastosowanie do określonej jednostki fizycznej, na przykład komputera lub do jednostki logicznej, takiej jak aplikacja lub witryna sieci Web. Określona jednostka logiczna może istnieć na komputerze lokalnym lub na serwerze zdalnym. Jeśli dla określonej jednostki nie istnieje plik konfiguracji, obiekt konfiguracji reprezentuje domyślne ustawienia konfiguracji zdefiniowane przez plik Machine. config.

Obiekt konfiguracji można uzyskać, korzystając z jednej z metod konfiguracji Open z następujących klas:

1. Klasa ConfigurationManager, jeśli jednostka jest aplikacją kliencką.
2. Klasa WebConfigurationManager, jeśli jednostka jest aplikacją sieci Web.

Te metody zwracają obiekt konfiguracji, który z kolei udostępnia wymagane metody i właściwości do obsługi źródłowych plików konfiguracji. Możesz uzyskać dostęp do tych plików do odczytu lub zapisu.

### <a name="reading"></a>Odczytu

Aby odczytać informacje o konfiguracji, należy użyć metody GetSection lub getsectioning. Użytkownik lub proces odczytywania musi mieć uprawnienia do odczytu wszystkich plików konfiguracji w hierarchii.

> [!NOTE]
> Jeśli używasz statycznej metody GetSection, która przyjmuje parametr path, parametr path musi odwoływać się do aplikacji, w której uruchomiono kod. W przeciwnym razie parametr jest ignorowany i są zwracane informacje o konfiguracji aktualnie uruchomionej aplikacji.

### <a name="writing"></a>Napisane

Aby zapisać informacje o konfiguracji, należy użyć jednej z metod zapisu. Użytkownik lub proces zapisu musi mieć uprawnienia do zapisu w pliku konfiguracji i katalogu na bieżącym poziomie hierarchii konfiguracji, a także uprawnienia do odczytu wszystkich plików konfiguracji w hierarchii.

Aby wygenerować plik konfiguracji, który reprezentuje ustawienia konfiguracji dziedziczone dla określonej jednostki, należy użyć jednej z następujących metod zapisu:

1. Zapisz metodę, aby utworzyć nowy plik konfiguracji.
2. Zapisz metodę, aby wygenerować nowy plik konfiguracji w innej lokalizacji.

## <a name="configuration-classes-and-namespaces"></a>Klasy konfiguracji i przestrzenie nazw

Wiele klas i metod konfiguracji jest podobnych do siebie. W poniższej tabeli opisano najczęściej używane klasy konfiguracji i przestrzenie nazw.

| **Klasa konfiguracji lub przestrzeń nazw** | **Opis** |
| --- | --- |
| Przestrzeń nazw [System. Configuration](https://msdn.microsoft.com/library/system.configuration.aspx) | Zawiera główne klasy konfiguracji dla wszystkich aplikacji .NET Framework. Klasy obsługi sekcji służą do uzyskiwania danych konfiguracyjnych sekcji z metod, takich jak GetSection i getsectioning. Te dwie metody nie są statyczne. |
| System. Configuration. Configuration — Klasa | Reprezentuje zestaw danych konfiguracyjnych dla komputera, aplikacji, katalogu sieci Web lub innego zasobu. Ta klasa zawiera przydatne metody, takie jak GetSection i getsections, do aktualizowania ustawień konfiguracji i uzyskiwania odwołań do sekcji i grup sekcji. Ta klasa jest używana jako typ zwracany dla metod, które uzyskują dane konfiguracji czasu projektowania, takie jak metody WebConfigurationManager i klasy ConfigurationManager. |
| System.Web.Configuration namespace | Zawiera klasy obsługi sekcji dla sekcji konfiguracyjnych ASP.NET zdefiniowanych w [ustawieniach konfiguracji ASP.NET](https://msdn.microsoft.com/library/b5ysx397.aspx). Klasy obsługi sekcji służą do uzyskiwania danych konfiguracyjnych sekcji z metod, takich jak GetSection i getsectioning. |
| System.Web.Configuration.WebConfigurationManager class | Zapewnia przydatne metody uzyskiwania odwołań do ustawień konfiguracji czasu wykonywania i czasu projektowania. Metody te używają klasy System. Configuration. Configuration jako typu zwracanego. Można użyć statycznej metody GetSection tej klasy lub niestatycznej metody GetSection klasy System. Configuration. ConfigurationManager zamiennie. W przypadku konfiguracji aplikacji sieci Web zaleca się używanie klasy System. Web. Configuration. WebConfigurationManager zamiast klasy System. Configuration. ConfigurationManager. |
| Przestrzeń nazw [System. Configuration. Provider](https://msdn.microsoft.com/library/system.configuration.provider.aspx) | Zapewnia sposób dostosowywania i zwiększania dostawcy konfiguracji. Jest to klasa podstawowa dla wszystkich klas dostawcy w systemie konfiguracyjnym. |
| Przestrzeń nazw [System. Web. Management](https://msdn.microsoft.com/library/system.web.management.aspx) | Zawiera klasy i interfejsy służące do zarządzania kondycją aplikacji sieci Web i monitorowania jej. Mówiąc ściślej, ta przestrzeń nazw nie jest uważana za część interfejsu API konfiguracji. Na przykład śledzenie i Wyzwalanie zdarzeń jest wykonywane przez klasy w tej przestrzeni nazw. |
| Przestrzeń nazw [System. Management. Instrumentation](https://msdn.microsoft.com/library/system.management.instrumentation.aspx) | Udostępnia klasy niezbędne dla Instrumentacji aplikacji, aby ujawniać informacje o zarządzaniu i zdarzenia za pomocą Instrumentacja zarządzania Windows (WMI) klientom potencjalnych klientów. Monitorowanie kondycji ASP.NET używa usługi WMI do dostarczania zdarzeń. Mówiąc ściślej, ta przestrzeń nazw nie jest uważana za część interfejsu API konfiguracji. |

## <a name="reading-from-aspnet-configuration-files"></a>Odczytywanie z plików konfiguracji ASP.NET

Klasa WebConfigurationManager jest podstawową klasą do odczytu z plików konfiguracji ASP.NET. Istnieją zasadniczo trzy kroki odczytu plików konfiguracji ASP.NET:

1. Pobierz obiekt konfiguracji przy użyciu metody OpenWebConfiguration.
2. Pobierz odwołanie do odpowiedniej sekcji w pliku konfiguracji.
3. Odczytaj wymagane informacje z pliku konfiguracyjnego.

Obiekt konfiguracji reprezentuje nie reprezentuje określonego pliku konfiguracji. Zamiast tego reprezentuje scalony widok konfiguracji komputera, aplikacji lub witryny sieci Web. Poniższy przykład kodu tworzy wystąpienie obiektu konfiguracji reprezentującego konfigurację aplikacji sieci Web o nazwie *productinfo*.

[!code-csharp[Main](configuration-and-instrumentation/samples/sample1.cs)]

> [!NOTE]
> Należy pamiętać, że jeśli ścieżka/ProductInfo nie istnieje, powyższy kod zwróci domyślną konfigurację określoną w pliku Machine. config.

Po utworzeniu obiektu konfiguracji można użyć metody GetSection lub getsectionobject, aby przejść do szczegółów ustawień konfiguracji. Poniższy przykład pobiera odwołanie do ustawień personifikacji dla powyższej aplikacji ProductInfo:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample2.cs)]

## <a name="writing-to-aspnet-configuration-files"></a>Zapisywanie w plikach konfiguracji ASP.NET

Podobnie jak w przypadku odczytywania z plików konfiguracji, Klasa WebConfigurationManager jest rdzeniem do zapisu w plikach konfiguracji Asp.NET. Istnieją również trzy kroki do zapisu w plikach konfiguracji ASP.NET.

1. Pobierz obiekt konfiguracji przy użyciu metody OpenWebConfiguration.
2. Pobierz odwołanie do odpowiedniej sekcji w pliku konfiguracji.
3. Zapisz wymagane informacje z pliku konfiguracji przy użyciu metody Save lub SaveAs.

Poniższy kod zmienia atrybut **Debug** elementu &lt;kompilacja&gt; na wartość false:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample3.cs)]

Po wykonaniu tego kodu atrybut **Debug** elementu &lt;kompilacja&gt; zostanie ustawiony na wartość false dla pliku Web. config aplikacji *webApp* .

## <a name="systemwebmanagement-namespace"></a>System.Web.Management Namespace

Przestrzeń nazw System. Web. Management zawiera klasy i interfejsy służące do monitorowania kondycji aplikacji ASP.NET oraz zarządzania nimi.

Rejestrowanie jest realizowane przez zdefiniowanie reguły, która kojarzy zdarzenia z dostawcą. Reguła definiuje typ zdarzeń wysyłanych do dostawcy. Następujące zdarzenia podstawowe są dostępne do zarejestrowania:

| **WebBaseEvent** | Podstawowa Klasa zdarzeń dla wszystkich zdarzeń. Zawiera właściwości wymagane dla wszystkich zdarzeń, takich jak kod zdarzenia, kod szczegółów zdarzenia, Data i godzina zgłoszenia zdarzenia, numer sekwencyjny, komunikat zdarzenia i szczegóły zdarzenia. |
| --- | --- |
| **WebManagementEvent** | Podstawowa Klasa zdarzeń dla zdarzeń zarządzania, taka jak okres istnienia aplikacji, żądanie, błąd i zdarzenia inspekcji. |
| **WebHeartbeatEvent** | Zdarzenie generowane przez aplikację w regularnych odstępach czasu, aby przechwycić przydatne informacje o stanie środowiska uruchomieniowego. |
| **WebAuditEvent** | Klasa bazowa dla zdarzeń inspekcji zabezpieczeń, która jest używana do oznaczania warunków, takich jak awaria autoryzacji, Niepowodzenie odszyfrowywania *itp.* |
| **WebRequestEvent** | Klasa bazowa dla wszystkich zdarzeń żądań informacyjnych. |
| **WebBaseErrorEvent** | Klasa bazowa dla wszystkich zdarzeń wskazujących na błędy. |

Dostępne typy dostawców umożliwiają wysyłanie danych wyjściowych zdarzenia do Podgląd zdarzeń, SQL Server, Instrumentacja zarządzania Windows (WMI) i poczty e-mail. Wstępnie skonfigurowane dostawcy i mapowania zdarzeń zmniejszają ilość pracy wymaganej do uzyskania zarejestrowanych danych wyjściowych zdarzenia.

ASP.NET 2,0 używa dostawcy dziennika zdarzeń, aby rejestrować zdarzenia na podstawie domen aplikacji uruchamianych i zatrzymywanych, a także rejestrowania wszelkich nieobsłużonych wyjątków. Pomaga to w zapewnieniu niektórych podstawowych scenariuszy. Załóżmy na przykład, że aplikacja zgłasza wyjątek, ale użytkownik nie zapisuje błędu i nie może go odtworzyć. Przy użyciu domyślnej reguły dziennika zdarzeń można zbierać informacje o wyjątkach i stosie, aby lepiej zrozumieć, jaki rodzaj błędu wystąpił. Inny przykład ma zastosowanie, jeśli aplikacja utraci stan sesji. W takim przypadku można poszukać w dzienniku zdarzeń, aby określić, czy domena aplikacji jest odtwarzana, oraz dlaczego domena aplikacji została zatrzymana w pierwszym miejscu.

Ponadto system monitorowania kondycji jest rozszerzalny. Można na przykład zdefiniować niestandardowe zdarzenia sieci Web, uruchomić je w aplikacji, a następnie zdefiniować regułę do wysyłania informacji o zdarzeniu do dostawcy, takiego jak Twój adres e-mail. Pozwala to łatwo powiązać instrumentację z dostawcami monitorowania kondycji. Innym przykładem może być zdarzenie wyzwalane za każdym razem, gdy zamówienie jest przetwarzane, i skonfigurować regułę, która wysyła każde zdarzenie do bazy danych SQL Server. Można również wywołać zdarzenie, gdy użytkownik nie może zalogować się wiele razy w wierszu i skonfigurować zdarzenie do korzystania z dostawców poczty e-mail.

Konfiguracja domyślnych dostawców i zdarzeń jest przechowywana w globalnym pliku Web. config. W pliku Global Web. config przechowywane są wszystkie ustawienia oparte na sieci Web, które zostały zapisane w pliku Machine. config w ASP.NET 1x. Globalny plik Web. config znajduje się w następującym katalogu:

`%windir%\Microsoft.Net\Framework\v2.0.*\config\Web.config`

Sekcja &lt;healthMonitoring&gt; pliku Global Web. config zawiera domyślne ustawienia konfiguracji. Można zastąpić te ustawienia lub skonfigurować własne ustawienia, implementując &lt;healthMonitoring&gt; sekcji w pliku Web. config aplikacji.

Sekcja &lt;healthMonitoring&gt; pliku Global Web. config zawiera następujące elementy:

| **udostępnia** | Zawiera dostawców skonfigurowanych dla Podgląd zdarzeń, usługi WMI i SQL Server. |
| --- | --- |
| **eventMappings** | Zawiera mapowania dla różnych klas webbase. Tę listę można rozwinąć w przypadku generowania własnej klasy zdarzeń. Generowanie własnej klasy zdarzeń zapewnia bardziej szczegółowy poziom szczegółowości dostawców, do których wysyłane są informacje. Można na przykład skonfigurować Nieobsłużone wyjątki do wysłania do SQL Server, wysyłając własne zdarzenia niestandardowe do poczty e-mail. |
| **przepisy** | Łączy eventMappings z dostawcą. |
| **buforowania** | Używany z dostawcami SQL Server i poczty e-mail, aby określić, jak często mają być opróżniane zdarzenia do dostawcy. |

Poniżej znajduje się przykładowy kod z globalnego pliku Web. config.

[!code-xml[Main](configuration-and-instrumentation/samples/sample4.xml)]

## <a name="how-to-store-events-to-event-viewer"></a>Jak przechowywać zdarzenia do Podgląd zdarzeń

Jak wspomniano wcześniej, dostawca rejestrowania zdarzeń w Podgląd zdarzeń jest skonfigurowany dla Ciebie w globalnym pliku Web. config. Domyślnie wszystkie zdarzenia oparte na **WebBaseErrorEvent** i **WebFailureAuditEvent** są rejestrowane. Można dodać dodatkowe reguły w celu zarejestrowania dodatkowych informacji w dzienniku zdarzeń. Na przykład jeśli chcesz rejestrować wszystkie zdarzenia (*tj.* każde zdarzenie oparte na **WebBaseEvent**), możesz dodać następującą regułę do pliku Web. config:

[!code-xml[Main](configuration-and-instrumentation/samples/sample5.xml)]

Ta reguła spowoduje połączenie mapy zdarzeń **wszystkie zdarzenia** z dostawcą dziennika zdarzeń. Zarówno tabela eventmapping, jak i dostawca są zawarte w globalnym pliku Web. config.

## <a name="how-to-store-events-to-sql-server"></a>Jak przechowywać zdarzenia do SQL Server

Ta metoda używa bazy danych **ASPNETDB** , która jest generowana za pomocą narzędzia ASPNET\_regsql. exe. Dostawca domyślny używa parametrów połączenia LocalSqlServer, które korzystają z bazy danych opartej na plikach w folderze danych\_lub w lokalnym wystąpieniu SQLExpress SQL Server. Parametry połączenia LocalSqlServer i są konfigurowane w globalnym pliku Web. config.

Parametry połączenia LocalSqlServer w globalnym pliku Web. config wyglądają następująco:

[!code-xml[Main](configuration-and-instrumentation/samples/sample6.xml)]

Jeśli chcesz użyć innego wystąpienia SQL Server, musisz użyć narzędzia ASPNET\_regsql. exe, które można znaleźć w%windir%\Microsoft.Net\Framework\v2.0. folder\*. Użyj narzędzia ASPNET\_regsql. exe, aby wygenerować niestandardową bazę danych **ASPNETDB** w wystąpieniu SQL Server, a następnie Dodaj parametry połączenia do pliku konfiguracji aplikacji, a następnie Dodaj dostawcę przy użyciu nowych parametrów połączenia. Po utworzeniu bazy danych **ASPNETDB** musisz ustawić regułę, aby połączyć tabela eventmapping z elementem SQL.

Bez względu na to, czy używasz domyślnego elementu SqlProvider, czy skonfigurować własnego dostawcę, musisz dodać regułę łączącą dostawcę z mapą zdarzeń. Poniższa reguła łączy nowego dostawcę utworzonego powyżej z mapą zdarzeń **wszystkie zdarzenia** . Ta zasada będzie rejestrować wszystkie zdarzenia w oparciu o **WebBaseEvent** i wysyłać je do MySqlWebEventProvider, które będą używać parametrów połączenia MYASPNETDB. Poniższy kod dodaje regułę, aby połączyć dostawcę z mapą zdarzeń:

[!code-xml[Main](configuration-and-instrumentation/samples/sample7.xml)]

Jeśli chcesz tylko wysłać błędy do SQL Server, możesz dodać następującą regułę:

[!code-xml[Main](configuration-and-instrumentation/samples/sample8.xml)]

## <a name="how-to-forward-events-to-wmi"></a>Jak przekazywać zdarzenia do usługi WMI

Możesz również przesłać zdarzenia do usługi WMI. Dostawca WMI jest domyślnie skonfigurowany dla użytkownika w globalnym pliku Web. config.

Poniższy przykład kodu dodaje regułę do przesyłania zdarzeń do usługi WMI:

[!code-xml[Main](configuration-and-instrumentation/samples/sample9.xml)]

Musisz dodać regułę, aby skojarzyć tabela eventmapping z dostawcą, a także aplikację odbiornika WMI do nasłuchiwania zdarzeń. Poniższy przykład kodu dodaje regułę, aby połączyć dostawcę WMI z mapą zdarzeń **wszystkie zdarzenia** :

[!code-xml[Main](configuration-and-instrumentation/samples/sample10.xml)]

## <a name="how-to-forward-events-to-email"></a>Jak przesłać dalej zdarzenia do poczty e-mail

Możesz również przesłać zdarzenia do poczty e-mail. Pamiętaj o tym, które reguły zdarzeń są mapowane na dostawcę poczty e-mail, ponieważ możesz przypadkowo wysłać do siebie wiele informacji, które mogą być lepiej dopasowane do SQL Server lub dziennika zdarzeń. Istnieją dwaj dostawcy poczty e-mail; SimpleMailWebEventProvider i TemplatedMailWebEventProvider. Każdy z nich ma te same atrybuty konfiguracji, z wyjątkiem atrybutów "template" i "detailedTemplateErrors", które są dostępne tylko w TemplatedMailWebEventProvider.

> [!NOTE]
> Żaden z tych dostawców poczty e-mail nie jest skonfigurowany. Należy dodać je do pliku Web. config.

Główną różnicą między tymi dwoma dostawcami poczty e-mail jest to, że SimpleMailWebEventProvider wysyła wiadomości e-mail w szablonie ogólnym, który nie może być modyfikowany. Przykładowy plik Web. config dodaje tego dostawcę poczty e-mail do listy skonfigurowanych dostawców przy użyciu następującej reguły:

[!code-xml[Main](configuration-and-instrumentation/samples/sample11.xml)]

Do powiązania dostawcy poczty e-mail z mapą zdarzeń **wszystkie zdarzenia** jest również dodawana następująca reguła:

[!code-xml[Main](configuration-and-instrumentation/samples/sample12.xml)]

## <a name="aspnet-20-tracing"></a>Śledzenie ASP.NET 2,0

Istnieją trzy główne ulepszenia śledzenia w programie ASP.NET 2,0.

1. Zintegrowana funkcja śledzenia
2. Dostęp programistyczny do komunikatów śledzenia
3. Ulepszone śledzenie na poziomie aplikacji

## <a name="integrated-tracing-functionality"></a>Zintegrowana funkcja śledzenia

Teraz można kierować komunikaty emitowane przez klasę System. Diagnostics. Trace do ASP.NET śledzenia danych wyjściowych i kierować komunikaty, które są emitowane przez śledzenie ASP.NET do System. Diagnostics. Trace. Możesz również przekazać zdarzenia Instrumentacji ASP.NET do usługi System. Diagnostics. Trace. Ta funkcja jest udostępniana przez nowy atrybut **writeToDiagnosticsTrace** elementu&gt; śledzenia &lt;. Gdy ta wartość logiczna ma wartość true, komunikaty śledzenia ASP.NET są przekazywane do infrastruktury śledzenia system. Diagnostics do użycia przez wszystkie detektory zarejestrowane do wyświetlania komunikatów śledzenia.

## <a name="programmatic-access-to-trace-messages"></a>Dostęp programistyczny do komunikatów śledzenia

ASP.NET 2,0 umożliwia programistyczny dostęp do wszystkich komunikatów śledzenia za pośrednictwem klasy **TraceContextRecord** i kolekcji **TraceRecords** . Najbardziej wydajnym sposobem uzyskiwania dostępu do komunikatów śledzenia jest zarejestrowanie delegata **TraceContextEventHandler** (nowość w ASP.NET 2,0) do obsługi nowego zdarzenia **TraceFinished** . Następnie możesz przechodzić przez komunikaty śledzenia w pętli.

Poniższy przykład kodu ilustruje:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample13.cs)]

W powyższym przykładzie pętlę przez kolekcję TraceRecords, a następnie napiszesz każdy komunikat do strumienia odpowiedzi.

## <a name="improved-application-level-tracing"></a>Ulepszone śledzenie na poziomie aplikacji

Śledzenie na poziomie aplikacji jest ulepszone poprzez wprowadzenie nowego atrybutu **mostRecent** elementu&gt; śledzenia &lt;. Ten atrybut określa, czy są wyświetlane najnowsze dane wyjściowe śledzenia na poziomie aplikacji, a starsze, przekraczające limity, które są wskazywane przez requestLimit, są odrzucane. W przypadku wartości false dane śledzenia są wyświetlane dla żądań, dopóki nie zostanie osiągnięty atrybut requestLimit.

## <a name="aspnet-command-line-tools"></a>Narzędzia wiersza polecenia ASP.NET

Istnieje kilka narzędzi wiersza polecenia, które ułatwiają konfigurację ASP.NET. Deweloperzy ASP.NET powinni znać narzędzie ASPNET\_regiis. exe. ASP.NET 2,0 udostępnia trzy inne narzędzia wiersza polecenia, które ułatwiają konfigurację.

Dostępne są następujące narzędzia wiersza polecenia:

| **Narzędzie** | **Korzystanie** |
| --- | --- |
| **ASPNET\_regiis. exe** | Umożliwia rejestrację ASP.NET za pomocą usług IIS. Istnieją dwie wersje tych narzędzi, które są dostarczane z ASP.NET 2,0, jeden dla systemów 32-bitowych (w folderze struktury) i jeden dla systemów 64-bitowych (w folderze Framework64). Wersja 64-bitowa nie zostanie zainstalowana na 32-bitowym systemie operacyjnym. |
| **ASPNET\_regsql. exe** | Narzędzie rejestracji SQL Server ASP.NET służy do tworzenia bazy danych Microsoft SQL Server do użycia przez dostawców SQL Server w programie ASP.NET lub do dodawania lub usuwania opcji z istniejącej bazy danych. Plik ASPNET\_regsql. exe znajduje się w folderze [dysk:] \WINDOWS\Microsoft.NET\Framework\versionNumber na serwerze sieci Web. |
| **ASPNET\_RegBrowsers. exe** | Narzędzie rejestracji w przeglądarce ASP.NET analizuje i kompiluje wszystkie definicje przeglądarki całego systemu w zestawie i instaluje zestaw w globalnej pamięci podręcznej zestawów. Narzędzie używa plików definicji przeglądarki (. Pliki przeglądarki) z podkatalogu przeglądarki .NET Framework. Narzędzie można znaleźć w katalogu%SystemRoot%\Microsoft.NET\Framework\version\. |
| **ASPNET\_Compiler. exe** | Narzędzie do kompilacji ASP.NET umożliwia skompilowanie aplikacji sieci Web ASP.NET w miejscu lub do wdrożenia w lokalizacji docelowej, takiej jak serwer produkcyjny. Kompilacja w miejscu ułatwia działanie aplikacji, ponieważ użytkownicy końcowi nie napotykają opóźnienia pierwszego żądania aplikacji podczas kompilowania aplikacji. |

Ponieważ ASPNET\_regiis. exe narzędzie nie jest nowe dla ASP.NET 2,0, nie będziemy omawiać tego w tym miejscu.

## <a name="aspnet-sql-server-registration-tool---aspnet_regsqlexe"></a>Narzędzie rejestracji SQL Server ASP.NET — ASPNET\_regsql. exe

Można ustawić kilka typów opcji za pomocą narzędzia rejestracji SQL Server ASP.NET. Można określić połączenie SQL, określić, które usługi aplikacji ASP.NET używają SQL Server do zarządzania informacjami, wskazać, która baza danych lub tabela jest używana na potrzeby zależności pamięci podręcznej SQL, a także dodać lub usunąć obsługę używania SQL Server do przechowywania procedur i stanu sesji.

Kilka usług aplikacji ASP.NET korzysta z dostawcy, aby zarządzać przechowywaniem i pobieraniem danych ze źródła danych. Każdy dostawca jest specyficzny dla źródła danych. ASP.NET zawiera dostawcę SQL Server dla następujących funkcji ASP.NET:

- Członkostwo (Klasa [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) ).
- Zarządzanie rolami (Klasa [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) ).
- Profil (Klasa [SqlProfileProvider](https://msdn.microsoft.com/library/system.web.profile.sqlprofileprovider.aspx) ).
- Personalizacja składniki Web Part (Klasa [SqlPersonalizationProvider](https://msdn.microsoft.com/library/system.web.ui.webcontrols.webparts.sqlpersonalizationprovider.aspx) ).
- Zdarzenia sieci Web (Klasa [SqlWebEventProvider](https://msdn.microsoft.com/library/system.web.management.sqlwebeventprovider.aspx) ).

Po zainstalowaniu ASP.NET plik Machine. config dla serwera zawiera elementy konfiguracji, które określają SQL Server dostawców dla każdej funkcji ASP.NET, które są zależne od dostawcy. Ci dostawcy są domyślnie skonfigurowani do łączenia się z wystąpieniem użytkownika lokalnego SQL Server Express 2005. Jeśli zmienisz domyślne parametry połączenia używane przez dostawców, zanim będzie można użyć dowolnych funkcji ASP.NET skonfigurowanych w konfiguracji komputera, należy zainstalować bazę danych SQL Server i elementy bazy danych dla wybranej funkcji za pomocą ASPNET\_regsql. exe. Jeśli baza danych określona za pomocą narzędzia rejestracji SQL jeszcze nie istnieje (aspnetdb będzie domyślną bazą danych, jeśli nie została określona w wierszu polecenia), bieżący użytkownik musi mieć uprawnienia do tworzenia baz danych w SQL Server oraz do tworzenia schematu e lements w bazie danych.

### <a name="sql-cache-dependency"></a>Zależność pamięci podręcznej SQL

Zaawansowana funkcja buforowania danych wyjściowych ASP.NET jest zależnością pamięci podręcznej SQL. Zależność pamięci podręcznej SQL obsługuje dwa różne tryby działania: jeden, który korzysta z ASP.NET implementacji sondowania tabeli i drugiego trybu, który używa funkcji powiadomień o zapytaniach w SQL Server 2005. Narzędzie rejestracji SQL może służyć do konfigurowania trybu sondowania tabeli.

### <a name="session-state"></a>Stan sesji

Domyślnie wartości i informacje o stanie sesji są przechowywane w pamięci w ramach procesu ASP.NET. Alternatywnie można przechowywać dane sesji w bazie danych SQL Server, która może być współużytkowana przez wiele serwerów sieci Web. Jeśli baza danych określona dla stanu sesji za pomocą narzędzia rejestracji SQL jeszcze nie istnieje, bieżący użytkownik musi mieć uprawnienia do tworzenia baz danych w SQL Server oraz do tworzenia elementów schematu w ramach bazy danych. Jeśli baza danych istnieje, bieżący użytkownik musi mieć uprawnienia do tworzenia elementów schematu w istniejącej bazie danych.

Aby zainstalować bazę danych stanu sesji na SQL Server, uruchom narzędzie ASPNET\_regsql. exe i podaj następujące informacje za pomocą polecenia:

- Nazwa wystąpienia SQL Server przy użyciu opcji **-S** .
- Poświadczenia logowania dla konta, które ma uprawnienia do tworzenia bazy danych na komputerze z uruchomionym SQL Server. Użyj opcji **-E** , aby użyć aktualnie zalogowanego użytkownika lub użyć opcji **-U** , aby określić identyfikator użytkownika wraz z opcją **-P** , aby określić hasło.
- Opcja wiersza polecenia **-ssadd** , aby dodać bazę danych stanu sesji.

Domyślnie nie można użyć ASPNET\_regsql. exe narzędzie do zainstalowania bazy danych stanu sesji na komputerze z systemem SQL Server 2005 Express Edition.

### <a name="the-aspnet-browser-registration-tool---aspnet_regbrowsersexe"></a>Narzędzie rejestracji przeglądarki ASP.NET — ASPNET\_RegBrowsers. exe

W programie ASP.NET w wersji 1,1 plik Machine. config zawiera sekcję o nazwie &lt;browserCaps&gt;. Ta sekcja zawiera serię wpisów XML, które definiuje konfiguracje dla różnych przeglądarek na podstawie wyrażenia regularnego. W przypadku ASP.NET w wersji 2,0 nowy. Plik przeglądarki definiuje parametry konkretnej przeglądarki przy użyciu wpisów XML. Informacje są dodawane do nowej przeglądarki poprzez dodanie nowej. Plik przeglądarki do folderu znajdującego się w%SystemRoot%\Microsoft.NET\Framework\version\CONFIG\Browsers w systemie.

Ponieważ aplikacja nie odczytuje pliku. config za każdym razem, gdy wymaga informacji przeglądarki, można utworzyć nową. Plik przeglądarki i uruchom ASPNET\_RegBrowsers. exe, aby dodać wymagane zmiany do zestawu. Dzięki temu serwer może natychmiast uzyskać dostęp do nowych informacji przeglądarki, aby nie trzeba było wyłączać żadnej aplikacji w celu pobrania informacji. Aplikacja może uzyskiwać dostęp do funkcji przeglądarki za pomocą właściwości Browser bieżącego żądania HttpRequest.

Następujące opcje są dostępne podczas uruchamiania ASPNET\_regbrowser. exe:

| **Opcja** | **Opis** |
| --- | --- |
| **-?** | Wyświetla tekst pomocy ASPNET\_regbbrowsers. exe w oknie wiersza polecenia. |
| **-i** | Tworzy zestaw możliwości przeglądarki środowiska uruchomieniowego i instaluje go w globalnej pamięci podręcznej zestawów. |
| **-u** | Odinstalowuje zestaw możliwości przeglądarki środowiska uruchomieniowego z globalnej pamięci podręcznej zestawów. |

## <a name="the-aspnet-compilation-tool---aspnet_compilerexe"></a>Narzędzie kompilacji ASP.NET — ASPNET\_Compiler. exe

Narzędzia do kompilacji ASP.NET można użyć na dwa sposoby: dla kompilacji w miejscu i kompilacji na potrzeby wdrożenia, gdzie określono docelowy katalog wyjściowy.

### <a name="compiling-an-application-in-place"></a>[Kompilowanie aplikacji w miejscu](https://msdn.microsoft.com/library/ms229863.aspx)

Narzędzie kompilacji ASP.NET może skompilować aplikację w miejscu, czyli naśladuje zachowanie wielu żądań do aplikacji, co spowoduje regularne Kompilowanie. Użytkownicy wstępnie skompilowanej witryny nie będą mieli opóźnień spowodowanych przez skompilowanie strony przy pierwszym żądaniu.

Podczas wstępnej kompilacji lokacji są stosowane następujące elementy:

- Lokacja zachowuje swoje pliki i strukturę katalogów.
- Muszą być dostępne kompilatory dla wszystkich języków programowania używanych przez lokację na serwerze programu.
- Jeśli dojdzie do niepowodzenia kompilacji, cała kompilacja kończy się niepowodzeniem.

Możesz również ponownie skompilować aplikację w miejscu po dodaniu do niej nowych plików źródłowych. Narzędzie kompiluje tylko nowe lub zmienione pliki, o ile nie zostanie uwzględniona opcja **-c** .

> [!NOTE]
> Kompilacja aplikacji zawierającej zagnieżdżoną aplikację nie kompiluje aplikacji zagnieżdżonej. Zagnieżdżona aplikacja musi być skompilowana osobno.

### <a name="compiling-an-application-for-deployment"></a>[Kompilowanie aplikacji do wdrożenia](https://msdn.microsoft.com/library/ms229863.aspx)

Kompilowanie aplikacji do wdrożenia (kompilacja do lokalizacji docelowej) przez określenie parametru targetDir. TargetDir może być ostateczną lokalizacją dla aplikacji sieci Web lub można również wdrożyć skompilowaną aplikację. Użycie opcji **-u** kompiluje aplikację w taki sposób, aby można było wprowadzać zmiany do określonych plików w skompilowanej aplikacji bez konieczności ich ponownego kompilowania. ASPNET\_Compiler. exe rozróżnia statyczne i dynamiczne typy plików i obsługuje je inaczej podczas tworzenia aplikacji.

- Statyczne typy plików to te, które nie mają skojarzonego kompilatora lub dostawcy kompilacji, takie jak pliki o nazwach z rozszerzeniami, takimi jak. CSS,. gif,. htm,. html,. jpg,. js i tak dalej. Te pliki są po prostu kopiowane do lokalizacji docelowej, a ich względne miejsca w strukturze katalogów są zachowywane.
- Dynamiczne typy plików to te, które mają skojarzony kompilator lub dostawca kompilacji, w tym pliki z rozszerzeniami nazw plików specyficznymi dla ASP.NET, takimi jak. asax,. ascx,. ashx,. aspx,. browser,. Master i tak dalej. Narzędzie kompilacji ASP.NET generuje zestawy z tych plików. Jeśli opcja **-u** zostanie pominięta, narzędzie tworzy również pliki z rozszerzeniem nazwy pliku. SKOMPILOWANe, które mapują oryginalne pliki źródłowe do ich zestawu. Aby upewnić się, że struktura katalogów źródła aplikacji jest zachowywana, narzędzie generuje pliki zastępcze w odpowiednich lokalizacjach w aplikacji docelowej.

Aby wskazać, że można modyfikować zawartość skompilowanej aplikacji, należy użyć opcji **-u** . W przeciwnym razie kolejne modyfikacje są ignorowane lub powodują błędy w czasie wykonywania.

W poniższej tabeli opisano, jak narzędzie kompilacji ASP.NET obsługuje różne typy plików, gdy jest dostępna opcja **-u** .

| **Typ pliku** | **Akcja kompilatora** |
| --- | --- |
| .ascx, .aspx, .master | Te pliki są podzielone na znaczniki i kod źródłowy, które obejmują zarówno pliki związane z kodem, jak i kod, który jest ujęty w &lt;skrypt runat = "Server"&gt; elementy. Kod źródłowy jest kompilowany do zestawów, z nazwami, które pochodzą z algorytmu wyznaczania wartości skrótu, a zestawy są umieszczane w katalogu bin. Każdy kod wbudowany, czyli kod ujęty między **&lt;%** i **%&gt;** nawiasy, jest dołączony do znaczników i nie został skompilowany. Nowe pliki o takiej samej nazwie jak pliki źródłowe są tworzone w taki sposób, aby zawierały oznakowanie i umieszczone w odpowiednich katalogach wyjściowych. |
| .ashx, .asmx | Te pliki nie są kompilowane i są przenoszone do katalogów wyjściowych, ponieważ nie są kompilowane. Jeśli chcesz, aby został skompilowany kod procedury obsługi, umieść kod w plikach kodu źródłowego w aplikacji\_katalogu kodu. |
| . cs,. vb,. JSL,. cpp (bez plików powiązanych z kodem dla wymienionych wcześniej typów plików) | Te pliki są kompilowane i uwzględniane jako zasoby w zestawach, które odwołują się do nich. Pliki źródłowe nie są kopiowane do katalogu wyjściowego. Jeśli plik kodu nie jest przywoływany, nie jest kompilowany. |
| Niestandardowe typy plików | Te pliki nie są kompilowane. Te pliki są kopiowane do odpowiednich katalogów wyjściowych. |
| Pliki kodu źródłowego w aplikacji\_podkatalogu kodu | Te pliki są kompilowane do zestawów i umieszczane w katalogu bin. |
| Pliki resx i. zasobów w aplikacji\_podkatalogu GlobalResources | Te pliki są kompilowane do zestawów i umieszczane w katalogu bin. W głównym katalogu wyjściowym nie jest tworzony żaden podkatalog aplikacji\_GlobalResources, w którym znajdują się pliki resx lub Resources znajdujące się w katalogu źródłowym. |
| Pliki resx i. zasobów w aplikacji\_podkatalogu LocalResources | Te pliki nie są kompilowane i są kopiowane do odpowiednich katalogów wyjściowych. |
| pliki. Skin w aplikacji\_podkatalogu motywy | Pliki skórki i statyczne pliki motywów nie są kompilowane i są kopiowane do odpowiednich katalogów wyjściowych. |
| Zestawy statycznych typów plików Web. config przeglądarki istnieją już w katalogu bin | Te pliki są kopiowane do katalogów danych wyjściowych. |

W poniższej tabeli opisano, jak narzędzie kompilacji ASP.NET obsługuje różne typy plików, gdy opcja **-u** jest pomijana.

| **Typ pliku** | **Akcja kompilatora** |
| --- | --- |
| .aspx, .asmx, .ashx, .master | Te pliki są podzielone na znaczniki i kod źródłowy, które obejmują zarówno pliki związane z kodem, jak i kod, który jest ujęty w &lt;skrypt runat = "Server"&gt; elementy. Kod źródłowy jest kompilowany do zestawów, z nazwami, które pochodzą z algorytmu wyznaczania wartości skrótu. Zestawy powstające są umieszczane w katalogu bin. Każdy kod wbudowany, czyli kod ujęty między **&lt;%** i **%&gt;** nawiasy, jest dołączony do znaczników i nie został skompilowany. Kompilator tworzy nowe pliki, aby zawierały adiustację o takiej samej nazwie jak pliki źródłowe. Te pliki będą umieszczane w katalogu bin. Kompilator tworzy również pliki o takiej samej nazwie jak pliki źródłowe, ale z rozszerzeniem. SKOMPILOWANe, które zawierają informacje dotyczące mapowania. Polu. SKOMPILOWANe pliki są umieszczane w katalogach wyjściowych odpowiadających oryginalnej lokalizacji plików źródłowych. |
| .ascx | Te pliki są podzielone na znaczniki i kod źródłowy. Kod źródłowy jest kompilowany do zestawów i umieszczony w katalogu bin, z nazwami, które pochodzą z algorytmu wyznaczania wartości skrótu. Nie Wygenerowano żadnych plików znaczników. |
| . cs,. vb,. JSL,. cpp (bez plików powiązanych z kodem dla wymienionych wcześniej typów plików) | Kod źródłowy, do którego odwołuje się zestawy generowane z plików. ascx,. ashx lub. aspx, jest kompilowany do zestawów i umieszczonych w katalogu bin. Żadne pliki źródłowe nie są kopiowane. |
| Niestandardowe typy plików | Te pliki są kompilowane jak pliki dynamiczne. W zależności od typu pliku, na którym się opiera, kompilator może umieścić pliki mapowania w katalogach wyjściowych. |
| Pliki w aplikacji\_podkatalogu kodu | Pliki kodu źródłowego w tym podkatalogu są kompilowane do zestawów i umieszczane w katalogu bin. |
| Pliki w aplikacji\_podkatalogu GlobalResources | Te pliki są kompilowane do zestawów i umieszczane w katalogu bin. W głównym katalogu wyjściowym nie jest tworzony żaden podkatalog\_aplikacji GlobalResources. Jeśli plik konfiguracji określa wartość appliesTo = "All", pliki resx i Resources są kopiowane do katalogów wyjściowych. Nie są one kopiowane, jeśli odwołują się do [BuildProvider](https://msdn.microsoft.com/library/system.web.configuration.buildprovider.aspx). |
| Pliki resx i. zasobów w aplikacji\_podkatalogu LocalResources | Te pliki są kompilowane do zestawów z unikatowymi nazwami i umieszczane w katalogu bin. Do katalogów wyjściowych nie są kopiowane pliki RESX ani zasobów. |
| pliki. Skin w aplikacji\_podkatalogu motywy | Motywy są kompilowane do zestawów i umieszczane w katalogu bin. Pliki szczątkowe są tworzone dla plików skór i umieszczane w odpowiednim katalogu wyjściowym. Pliki statyczne (takie jak. CSS) są kopiowane do katalogów wyjściowych. |
| Zestawy statycznych typów plików Web. config przeglądarki istnieją już w katalogu bin | Te pliki są kopiowane do katalogu wyjściowego. |

### <a name="fixed-assembly-names"></a>[Stałe nazwy zestawów](https://msdn.microsoft.com/library/ms229863.aspx##)

Niektóre scenariusze, takie jak wdrażanie aplikacji sieci Web przy użyciu Instalator Windows MSI, wymagają użycia spójnych nazw plików i zawartości, a także spójnych struktur katalogów do identyfikowania zestawów lub ustawień konfiguracji aktualizacji. W takich przypadkach można użyć opcji **-fixednames** , aby określić, że narzędzie kompilacji ASP.NET powinno kompilować zestaw dla każdego pliku źródłowego, zamiast korzystać z lokalizacji, w której wiele stron jest kompilowanych do zestawów. Może to prowadzić do dużej liczby zestawów, więc Jeśli obawiasz się skalowalności, należy użyć tej opcji z zachowaniem ostrożności.

### <a name="strong-name-compilation"></a>[Kompilacja o silnej nazwie](https://msdn.microsoft.com/library/ms229863.aspx##)

Dostępne są opcje **-APTCA**, **-delaysign**i **-KeyFile** **, aby** można było używać ASPNET\_Compiler. exe do tworzenia silnie nazwanych zestawów bez użycia [Narzędzia silnej nazwy (SN. exe)](https://msdn.microsoft.com/library/k5b5tt23.aspx) . Te opcje odpowiadają odpowiednio, **AllowPartiallyTrustedCallersAttribute**, **AssemblyDelaySignAttribute**, **AssemblyKeyNameAttribute**i **AssemblyKeyFileAttribute**.

Dyskusje tych atrybutów wykraczają poza zakres tego kursu.

## <a name="labs"></a>Laboratoria

Każda z poniższych laboratoriów korzysta z poprzednich laboratoriów. Należy je wykonać w pożądanej kolejności.

## <a name="lab-1-using-the-configuration-api"></a>Lab 1: korzystanie z interfejsu API konfiguracji

1. Utwórz nową witrynę sieci Web o nazwie *mod9lab*.
2. Dodaj nowy plik konfiguracji sieci Web do lokacji.
3. Dodaj następujący plik do pliku Web. config:

[!code-xml[Main](configuration-and-instrumentation/samples/sample14.xml)]

Dzięki temu masz uprawnienia do zapisywania zmian w pliku Web. config.

1. Dodaj nową kontrolkę etykieta do default. aspx i zmień identyfikator na **lblDebugStatus**.
2. Dodaj nową kontrolkę Button do default. aspx.
3. Zmień identyfikator kontrolki Button na **btnToggleDebug** i tekst, aby **przełączyć stan debugowania**.
4. Otwórz widok kodu dla pliku związanego z kodem default. aspx i Dodaj instrukcję **using** dla elementu **System. Web. Configuration** w następujący sposób:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample15.cs)]

1. Dodaj dwie zmienne prywatne do klasy i metodę\_init, jak pokazano poniżej:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample16.cs)]

1. Dodaj następujący kod do strony\_obciążenie:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample17.cs)]

1. Zapisz i Przeglądaj default. aspx. Zauważ, że w kontrolce etykieta jest wyświetlany bieżący stan debugowania.
2. Kliknij dwukrotnie formant Button w Projektancie i Dodaj następujący kod do zdarzenia kliknięcia dla kontrolki Button:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample18.cs)]

1. Zapisz i Przeglądaj default. aspx, a następnie kliknij przycisk.
2. Otwórz plik Web. config po każdym kliknięciu przycisku i obserwuj atrybut **Debug** w sekcji &lt;kompilacja&gt;.

## <a name="lab-2-logging-application-restarts"></a>Lab 2: rejestrowanie ponownych uruchomień aplikacji

W tym laboratorium utworzysz kod, który umożliwi przełączenie rejestrowania zamknięć aplikacji, uruchamiania i ponownych kompilacji w Podgląd zdarzeń.

1. Dodaj DropDownList do default. aspx i zmień identyfikator na ddlLogAppEvents.
2. Ustaw właściwość **Autoogłaszania zwrotnego** dla DropDownList na **true**.
3. Dodaj trzy elementy do kolekcji Items dla DropDownList. Wprowadź **tekst** dla pierwszego elementu *Wybierz wartość* i wartość-1. Wprowadź **tekst** i **wartość** drugiego elementu **true** oraz **tekst** i **wartość** trzeciego elementu **false**.
4. Dodaj nową etykietę do default. aspx. Zmień identyfikator na **lblLogAppEvents**.
5. Otwórz widok związany z kodem dla default. aspx i Dodaj nową deklarację dla zmiennej typu HealthMonitoringSection, jak pokazano poniżej:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample19.cs)]

1. Dodaj następujący kod do istniejącego kodu na stronie\_init:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample20.cs)]

1. Kliknij dwukrotnie DropDownList i Dodaj następujący kod do zdarzenia SelectedIndexChanged.:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample21.cs)]

1. Przeglądaj default. aspx.
2. Ustaw listę rozwijaną na **false**.
3. Wyczyść dziennik aplikacji w Podgląd zdarzeń.
4. Kliknij przycisk, aby zmienić atrybut Debug dla aplikacji.
5. Odśwież dziennik aplikacji w Podgląd zdarzeń. 

    1. Czy jakieś zdarzenia były zarejestrowane?
    2. Dlaczego lub dlaczego nie?
6. Ustaw listę rozwijaną na **wartość true.**
7. Kliknij przycisk, aby przełączyć atrybut Debug dla aplikacji.
8. Odświeżanie aplikacji Zaloguj Podgląd zdarzeń. 

    1. Czy jakieś zdarzenia były zarejestrowane?
    2. Jaki był powód zamknięcia aplikacji?
9. Eksperymentowanie z włączaniem i wyłączaniem rejestrowania oraz sprawdzanie zmian wprowadzonych w pliku Web. config.

## <a name="more-information"></a>Więcej informacji:

Model dostawcy ASP.NET 2.0 umożliwia tworzenie własnych dostawców dla nie tylko instrumentacji aplikacji, ale również za wiele innych zastosowań, takich jak członkostwo, profile itd. Aby uzyskać szczegółowe informacje na temat pisania niestandardowego dostawcy do rejestrowania zdarzeń aplikacji w pliku tekstowym, odwiedź [ten link](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/ASPNETProvMod_Prt6.asp).

---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-vb
title: Rejestrowanie szczegółów błędu za pomocą monitorowania kondycji ASP.NET (VB) | Microsoft Docs
author: rick-anderson
description: System monitorowania kondycji firmy Microsoft zapewnia łatwy i dostosowywalny sposób rejestrowania różnych zdarzeń sieci Web, w tym nieobsłużonych wyjątków. Ten samouczek przeprowadzi Cię przez prz...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: 09a6c74e-936a-4c04-8547-5bb313a4e4a3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-vb
msc.type: authoredcontent
ms.openlocfilehash: f57aca41771adfd9a7c7f38da1916db9197262da
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74587787"
---
# <a name="logging-error-details-with-aspnet-health-monitoring-vb"></a>Rejestrowanie szczegółów błędu za pomocą monitorowania kondycji ASP.NET (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_13_VB.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial13_HealthMonitoring_vb.pdf)

> System monitorowania kondycji firmy Microsoft zapewnia łatwy i dostosowywalny sposób rejestrowania różnych zdarzeń sieci Web, w tym nieobsłużonych wyjątków. Ten samouczek przeprowadzi Cię przez proces konfigurowania systemu monitorowania kondycji w celu rejestrowania nieobsłużonych wyjątków w bazie danych i powiadamiania deweloperów za pośrednictwem wiadomości e-mail.

## <a name="introduction"></a>Wprowadzenie

Rejestrowanie to przydatne narzędzie do monitorowania kondycji wdrożonej aplikacji oraz do diagnozowania wszelkich problemów, które mogą wystąpić. Jest to szczególnie ważne, aby rejestrować błędy występujące we wdrożonej aplikacji, dzięki czemu można je rozwiązać. Zdarzenie `Error` jest zgłaszane, gdy wystąpi nieobsługiwany wyjątek w aplikacji ASP.NET. w [poprzednim samouczku](processing-unhandled-exceptions-vb.md) przedstawiono sposób powiadamiania dewelopera o błędzie i rejestrowania jego szczegółów przez utworzenie programu obsługi zdarzeń dla zdarzenia `Error`. Jednak utworzenie procedury obsługi zdarzeń `Error` w celu zarejestrowania szczegółów błędu i powiadomienia dewelopera jest niezbędna, ponieważ to zadanie może być wykonywane przez ASP. *System monitorowania kondycji*sieci.

System monitorowania kondycji został wprowadzony w ASP.NET 2,0 i jest przeznaczony do monitorowania kondycji wdrożonej aplikacji ASP.NET przez rejestrowanie zdarzeń występujących w okresie istnienia aplikacji lub żądania. Zdarzenia zarejestrowane przez system monitorowania kondycji są określane jako *zdarzenia monitorowania kondycji* lub *zdarzenia sieci Web*i obejmują:

- Zdarzenia okresu istnienia aplikacji, takie jak uruchomienie lub zatrzymanie aplikacji
- Zdarzenia zabezpieczeń, w tym nieudane próby logowania i Nieudane żądania autoryzacji adresu URL
- Błędy aplikacji, w tym Nieobsłużone wyjątki, Wyświetlanie wyjątków analizy stanu, wyjątki walidacji żądań i błędy kompilacji, między innymi typami błędów.

Po podniesieniu zdarzenia monitorowania kondycji można je zalogować do dowolnej liczby określonych *źródeł dzienników*. System monitorowania kondycji jest dostarczany ze źródłami dzienników, które rejestrują zdarzenia sieci Web w bazie danych Microsoft SQL Server, w dzienniku zdarzeń systemu Windows lub za pośrednictwem wiadomości e-mail, między innymi. Możesz również utworzyć własne źródła dzienników.

Zdarzenia dzienników systemu monitorowania kondycji wraz z użytymi źródłami dzienników są zdefiniowane w `Web.config`. Za pomocą kilku wierszy znaczników konfiguracji można użyć monitorowania kondycji, aby rejestrować wszystkie Nieobsłużone wyjątki do bazy danych i powiadamiać o wyjątku za pośrednictwem poczty e-mail.

## <a name="exploring-the-health-monitoring-systems-configuration"></a>Eksplorowanie konfiguracji systemu monitorowania kondycji

Zachowanie systemu monitorowania kondycji jest definiowane przez informacje o konfiguracji, które znajdują się w [elemencie`<healthMonitoring>`](https://msdn.microsoft.com/library/2fwh2ss9.aspx) w `Web.config`. Ta sekcja konfiguracji definiuje między innymi następujące trzy ważne informacje:

1. Zdarzenia monitorowania kondycji, które zostały zgłoszone, powinny być rejestrowane,
2. Źródła dzienników i
3. Jak każde zdarzenie monitorowania kondycji zdefiniowane w (1) jest zamapowane na źródła dzienników zdefiniowane w (2).

Te informacje są określone za poorednictwem trzech elementów konfiguracji podrzędnych: odpowiednio [`<eventMappings>`](https://msdn.microsoft.com/library/yc5yk01w.aspx), [`<providers>`](https://msdn.microsoft.com/library/zaa41kz1.aspx)i [`<rules>`](https://msdn.microsoft.com/library/fe5wyxa0.aspx).

Informacje o konfiguracji domyślnej systemu monitorowania kondycji znajdują się w pliku `Web.config` w `%WINDIR%\Microsoft.NET\Framework\version\CONFIG` folderze. Te domyślne informacje o konfiguracji z usuniętym znacznikiem dla zwięzłości są przedstawione poniżej:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample1.xml)]

Zdarzenia monitorowania kondycji są zdefiniowane w `<eventMappings>` elementu, który daje przyjazną nazwę dla klasy zdarzeń monitorowania kondycji. W powyższym znaczniku element `<eventMappings>` przypisuje przyjazną dla człowieka nazwę "wszystkie błędy" do zdarzeń monitorowania kondycji typu `WebBaseErrorEvent` i nazwę "inspekcje niepowodzeń" do zdarzeń monitorowania kondycji typu `WebFailureAuditEvent`.

Element `<providers>` definiuje źródła dzienników, dając im przyjazną nazwę i określając wszystkie informacje o konfiguracji specyficzne dla źródła dziennika. Pierwszy `<add>` element definiuje dostawcę "EventLogProvider", który rejestruje określone zdarzenia monitorowania kondycji przy użyciu klasy `EventLogWebEventProvider`. Klasa `EventLogWebEventProvider` rejestruje zdarzenie w dzienniku zdarzeń systemu Windows. Drugi `<add>` element definiuje dostawcę "SqlWebEventProvider", który rejestruje zdarzenia do bazy danych Microsoft SQL Server za pośrednictwem klasy `SqlWebEventProvider`. Konfiguracja "SqlWebEventProvider" określa parametry połączenia bazy danych (`connectionStringName`) między innymi opcjami konfiguracji.

Element `<rules>` mapuje zdarzenia określone w elemencie `<eventMappings>`, aby rejestrować źródła w `<providers>` elemencie. Domyślnie aplikacje sieci Web ASP.NET rejestrują wszystkie Nieobsłużone wyjątki i błędy inspekcji w dzienniku zdarzeń systemu Windows.

## <a name="logging-events-to-a-database"></a>Rejestrowanie zdarzeń w bazie danych

Domyślną konfigurację systemu monitorowania kondycji można dostosować w zależności od aplikacji sieci Web, dodając sekcję `<healthMonitoring>` do pliku `Web.config` aplikacji. Można uwzględnić dodatkowe elementy w sekcjach `<eventMappings>`, `<providers>`i `<rules>` przy użyciu elementu `<add>`. Aby usunąć ustawienie z konfiguracji domyślnej, użyj elementu `<remove>` lub użyj `<clear />`, aby usunąć wszystkie wartości domyślne z jednej z tych sekcji. Skonfigurujmy aplikację sieci Web przeglądający książki, aby rejestrować wszystkie Nieobsłużone wyjątki do bazy danych Microsoft SQL Server przy użyciu klasy `SqlWebEventProvider`.

Klasa `SqlWebEventProvider` jest częścią systemu monitorowania kondycji i rejestruje zdarzenie monitorowania kondycji w określonej SQL Server bazie danych. Klasa `SqlWebEventProvider` oczekuje, że określona baza danych zawiera procedurę przechowywaną o nazwie `aspnet_WebEvent_LogEvent`. Ta procedura składowana przekazuje szczegółowe informacje o zdarzeniu i jest z nim wykonywane zadanie przechowywania szczegółów zdarzenia. Dobrą wiadomośćą jest to, że nie trzeba tworzyć tej procedury składowanej ani tabeli do przechowywania szczegółów zdarzenia. Te obiekty można dodać do bazy danych za pomocą narzędzia `aspnet_regsql.exe`.

> [!NOTE]
> Narzędzie `aspnet_regsql.exe` zostało omówione z powrotem w temacie [ *Konfigurowanie witryny sieci Web używającej usługi aplikacji* samouczka,](configuring-a-website-that-uses-application-services-vb.md) gdy dodaliśmy obsługę środowiska ASP. Usługi aplikacji sieci. W związku z tym baza danych witryny sieci Web przeglądający książkę zawiera już `aspnet_WebEvent_LogEvent` procedury składowanej, która przechowuje informacje o zdarzeniu w tabeli o nazwie `aspnet_WebEvent_Events`.

Gdy masz niezbędną procedurę składowaną i tabelę dodaną do bazy danych, to wszystko, co pozostanie, powoduje, że monitorowanie kondycji rejestruje wszystkie Nieobsłużone wyjątki w bazie danych. W tym celu Dodaj następujące znaczniki do pliku `Web.config` witryny sieci Web:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample2.xml)]

Powyższy znacznik konfiguracji monitorowania kondycji używa `<clear />` elementów do czyszczenia wstępnie zdefiniowanych informacji konfiguracyjnych monitorowania kondycji z sekcji `<eventMappings>`, `<providers>`i `<rules>`. Następnie dodaje jeden wpis do każdej z tych sekcji.

- Element `<eventMappings>` definiuje pojedyncze zdarzenie monitorowania kondycji o nazwie "wszystkie błędy", które jest zgłaszane w przypadku wystąpienia nieobsługiwanego wyjątku.
- Element `<providers>` definiuje pojedyncze Źródło dziennika o nazwie "SqlWebEventProvider", które używa klasy `SqlWebEventProvider`. Atrybut `connectionStringName` został ustawiony na wartość "ReviewsConnectionString", czyli nazwę naszych parametrów połączenia zdefiniowanych w sekcji `<connectionStrings>`.
- Na koniec &lt;reguł&gt; element wskazuje, że gdy zdarzenie "wszystkie błędy" transpires, że powinno być rejestrowane przy użyciu dostawcy "SqlWebEventProvider".

Te informacje o konfiguracji instruują system monitorowania kondycji, aby rejestrował wszystkie Nieobsłużone wyjątki do bazy danych przeglądów książki.

> [!NOTE]
> Zdarzenie `WebBaseErrorEvent` jest wywoływane tylko w przypadku błędów serwera; nie zostało zgłoszone do błędów HTTP, takich jak żądanie dla zasobu ASP.NET, którego nie znaleziono. Różni się to od zachowania zdarzenia `Error` klasy `HttpApplication`, które jest zgłaszane dla błędów serwera i HTTP.

Aby wyświetlić system monitorowania kondycji w działaniu, odwiedź witrynę sieci Web i Wygeneruj błąd czasu wykonania, odwiedzając `Genre.aspx?ID=foo`. Powinna zostać wyświetlona odpowiednia strona błędu — szczegóły wyjątku żółtego ekranu zgonu (podczas odwiedzania lokalnego) lub strony błędu niestandardowego (podczas odwiedzania witryny w środowisku produkcyjnym). W tle system monitorowania kondycji zarejestrował informacje o błędzie w bazie danych. W tabeli `aspnet_WebEvent_Events` powinien znajdować się jeden rekord (patrz **rysunek 1**); Ten rekord zawiera informacje o błędzie środowiska uruchomieniowego, który właśnie wystąpił.

[![](logging-error-details-with-asp-net-health-monitoring-vb/_static/image2.png)](logging-error-details-with-asp-net-health-monitoring-vb/_static/image1.png)

**Rysunek 1**. szczegóły błędu zostały zarejestrowane w tabeli `aspnet_WebEvent_Events`  
([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](logging-error-details-with-asp-net-health-monitoring-vb/_static/image3.png))

### <a name="displaying-the-error-log-in-a-web-page"></a>Wyświetlanie dziennika błędów na stronie sieci Web

W przypadku bieżącej konfiguracji witryny sieci Web system monitorowania kondycji rejestruje wszystkie Nieobsłużone wyjątki w bazie danych. Jednak monitorowanie kondycji nie udostępnia żadnego mechanizmu wyświetlania dziennika błędów przez stronę sieci Web. Można jednak utworzyć stronę ASP.NET, która wyświetla te informacje z bazy danych. (W miarę jak zobaczysz chwilę, możesz wybrać opcję wysłania szczegółów błędu w wiadomości e-mail).

W przypadku utworzenia takiej strony należy wykonać kroki, aby zezwolić tylko autoryzowanym użytkownikom na wyświetlanie szczegółów błędu. Jeśli witryna korzysta już z kont użytkowników, można użyć reguł autoryzacji adresów URL, aby ograniczyć dostęp do strony do określonych użytkowników lub ról. Aby uzyskać więcej informacji na temat udzielania lub ograniczania dostępu do stron sieci Web na podstawie zalogowanego użytkownika, zapoznaj się z [samouczkami dotyczącymi zabezpieczeń w witrynie sieci Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

> [!NOTE]
> W kolejnym samouczku przedstawiono alternatywny dziennik błędów i system powiadomień o nazwie ELMAH. Program ELMAH zawiera wbudowany mechanizm wyświetlania dziennika błędów zarówno ze strony sieci Web, jak i kanału informacyjnego RSS.

## <a name="logging-events-to-email"></a>Rejestrowanie zdarzeń w wiadomości E-mail

System monitorowania kondycji zawiera dostawcę źródła dziennika, który "rejestruje" zdarzenie w wiadomości e-mail. Źródło dziennika zawiera te same informacje, które są rejestrowane w bazie danych w treści wiadomości e-mail. To źródło dziennika służy do powiadamiania dewelopera o wystąpieniu określonego zdarzenia monitorowania kondycji.

Zaktualizujmy konfigurację witryny sieci Web przeglądów książek, aby otrzymywać wiadomości e-mail za każdym razem, gdy wystąpi wyjątek. Aby to osiągnąć, musimy wykonać trzy zadania:

1. Skonfiguruj aplikację sieci Web ASP.NET do wysyłania wiadomości e-mail. W tym celu należy określić sposób wysyłania wiadomości e-mail za pośrednictwem `<system.net>` elementu konfiguracji. Aby uzyskać więcej informacji na temat wysyłania wiadomości e-mail w aplikacji ASP.NET, odwołaj się do [wysyłania wiadomości e-mail w ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx) i [System .NET. mail — często zadawane pytania](http://systemnetmail.com/).
2. Zarejestruj dostawcę źródła dziennika poczty e-mail w elemencie `<providers>` i
3. Dodaj wpis do elementu `<rules>`, który mapuje zdarzenie "wszystkie błędy" do dostawcy źródła dziennika dodanego w kroku (2).

System monitorowania kondycji obejmuje dwie klasy dostawcy źródła dziennika poczty e-mail: `SimpleMailWebEventProvider` i `TemplatedMailWebEventProvider`. [Klasa`SimpleMailWebEventProvider`](https://msdn.microsoft.com/library/system.web.management.simplemailwebeventprovider.aspx) wysyła wiadomość e-mail w postaci zwykłego tekstu, która zawiera szczegóły zdarzenia i zapewnia niewielkie dostosowanie treści wiadomości e-mail. Za pomocą [klasy`TemplatedMailWebEventProvider`](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx) należy określić stronę ASP.NET, której renderowane znaczniki służy jako treść wiadomości e-mail. [Klasa`TemplatedMailWebEventProvider`](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx) zapewnia znacznie większą kontrolę nad zawartością i formatem wiadomości e-mail, ale wymaga nieco większej liczby zadań z góry, aby utworzyć stronę ASP.NET, która generuje treść wiadomości e-mail. Ten samouczek koncentruje się na korzystaniu z klasy `SimpleMailWebEventProvider`.

Zaktualizuj element `<providers>` systemu monitorowania kondycji w pliku `Web.config`, aby uwzględnić Źródło dziennika dla klasy `SimpleMailWebEventProvider`:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample3.xml)]

Powyższy znacznik używa klasy `SimpleMailWebEventProvider` jako dostawcy źródła dzienników i przypisuje mu przyjazną nazwę "EmailWebEventProvider". Ponadto atrybut `<add>` zawiera dodatkowe opcje konfiguracji, takie jak do i z adresów wiadomości e-mail.

Zdefiniowano Źródło dziennika poczty e-mail, które pozostanie, że system monitorowania kondycji ma korzystać z tego źródła do nieobsłużonych wyjątków "log". W tym celu należy dodać nową regułę w sekcji `<rules>`:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample4.xml)]

Sekcja `<rules>` zawiera teraz dwie reguły. Pierwszy z nich o nazwie "wszystkie błędy do poczty E-mail" wysyła wszystkie Nieobsłużone wyjątki do źródła dziennika "EmailWebEventProvider". Ta reguła ma wpływ na wysyłanie szczegółowych informacji o błędach w witrynie sieci Web do określonego adresu. Zasada "wszystkie błędy do bazy danych" rejestruje szczegóły błędu w bazie danych lokacji. W związku z tym zawsze, gdy wystąpi nieobsługiwany wyjątek w lokacji, jego szczegóły są rejestrowane w bazie danych i wysyłane na określony adres e-mail.

**Rysunek 2** przedstawia wiadomość e-mail wygenerowaną przez klasę `SimpleMailWebEventProvider` podczas odwiedzania `Genre.aspx?ID=foo`.

[![](logging-error-details-with-asp-net-health-monitoring-vb/_static/image5.png)](logging-error-details-with-asp-net-health-monitoring-vb/_static/image4.png)

**Rysunek 2**. szczegóły błędu są wysyłane w wiadomości e-mail  
([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](logging-error-details-with-asp-net-health-monitoring-vb/_static/image6.png))

## <a name="summary"></a>Podsumowanie

System monitorowania kondycji ASP.NET został zaprojektowany tak, aby umożliwić administratorom monitorowanie kondycji wdrożonej aplikacji sieci Web. Zdarzenia monitorowania kondycji są wywoływane, gdy pewne akcje unfold, takie jak gdy aplikacja zostanie zatrzymana, gdy użytkownik pomyślnie zaloguje się do lokacji lub wystąpił nieobsługiwany wyjątek. Te zdarzenia mogą być rejestrowane w dowolnej liczbie źródeł dzienników. W tym samouczku pokazano, jak rejestrować szczegóły nieobsłużonych wyjątków do bazy danych i za pośrednictwem wiadomości e-mail.

Ten samouczek koncentruje się na korzystaniu z monitorowania kondycji w celu rejestrowania nieobsłużonych wyjątków, ale należy pamiętać, że monitorowanie kondycji zaprojektowano w celu mierzenia ogólnej kondycji wdrożonej aplikacji ASP.NET i obejmuje mnóstwo zdarzeń monitorowania kondycji i źródeł dzienników. zbadano tutaj. Co więcej, możesz utworzyć własne zdarzenia monitorowania kondycji i źródła dzienników, jeśli zajdzie taka potrzeba. Jeśli chcesz dowiedzieć się więcej o monitorowaniu kondycji, dobrym pierwszym krokiem jest zapoznanie się z artykułem [monitorowanie kondycji](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)programu [Erik Reitan](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx). W tym celu należy zapoznać się z [tematem jak: użyć monitorowania kondycji w programie ASP.NET 2,0](https://msdn.microsoft.com/library/ms998306.aspx).

Szczęśliwe programowanie!

### <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Monitorowanie kondycji ASP.NET — Omówienie](https://msdn.microsoft.com/library/bb398933.aspx)
- [Konfigurowanie i Dostosowywanie systemu monitorowania kondycji programu ASP.NET](http://dotnetslackers.com/articles/aspnet/ConfiguringAndCustomizingTheHealthMonitoringSystemOfASPNET.aspx)
- [Często zadawane pytania — monitorowanie kondycji w ASP.NET 2,0](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)
- [Instrukcje: wysyłanie wiadomości E-Mail na potrzeby powiadomień dotyczących monitorowania kondycji](https://msdn.microsoft.com/library/ms227553.aspx)
- [Instrukcje: korzystanie z monitorowania kondycji w ASP.NET](https://msdn.microsoft.com/library/ms998306.aspx)
- [Monitorowanie kondycji w ASP.NET](http://aspnet.4guysfromrolla.com/articles/031407-1.aspx)

> [!div class="step-by-step"]
> [Poprzednie](processing-unhandled-exceptions-vb.md)
> [dalej](logging-error-details-with-elmah-vb.md)

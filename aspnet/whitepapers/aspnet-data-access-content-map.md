---
uid: whitepapers/aspnet-data-access-content-map
title: ASP.NET dostęp do danych — zalecane zasoby | Microsoft Docs
author: rick-anderson
description: Ten temat zawiera linki do zasobów dokumentacji dotyczące uzyskiwania dostępu do danych w aplikacjach sieci Web ASP.NET, głównie przy użyciu Entity Framework i języka SQL SE...
ms.author: riande
ms.date: 07/25/2013
ms.assetid: f8157be1-4ab9-469e-ad3a-0ccc80b56c00
msc.legacyurl: /whitepapers/aspnet-data-access-content-map
msc.type: content
ms.openlocfilehash: 357851f195bf233c7c34a32bd156e4408d3e1b24
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78633134"
---
# <a name="aspnet-data-access---recommended-resources"></a>Dostęp do danych na platformie ASP.NET — zalecane zasoby

> Ten temat zawiera linki do zasobów dokumentacji dotyczących sposobu uzyskiwania dostępu do danych w aplikacjach sieci Web ASP.NET, głównie przy użyciu Entity Framework i SQL Server.
> 
> Jeśli znasz dobry wpis w blogu, wątek [StackOverflow](http://stackoverflow.com) lub inny link, który będzie przydatny, Wyślij do [nas wiadomość e-mail](mailto:aspnetue@microsoft.com?subject=Data Access Content Map) z linkiem.
> 
> Ostatnia aktualizacja 4/3/2014

Temat zawiera następujące sekcje:

- [Wprowadzenie z dostępem do danych w ASP.NET](#gettingstarted)
- [Korzystanie z Entity Framework](#ef)

    - [Korzystanie z Entity Framework Code First](#cf)
    - [Używanie migracje Code First platformy Entity Framework](#efcfmigrations)
    - [Korzystanie z Entity Framework Database First lub Model First (Projektant EF)](#efdbf)
    - [Ładowanie powiązanych danych w Entity Framework (ładowanie z opóźnieniem, ładowanie eager i jawne ładowanie)](#efrelateddata)
    - [Optymalizacja wydajności Entity Framework](#optimizingef)
    - [Obsługa współbieżności w aplikacji Entity Framework](#efconcurrency)
    - [Książki dotyczące Entity Framework](#efbooks)
    - [Dodatkowe zasoby Entity Framework](#otherefresources)
- [Powiązanie danych w aplikacjach formularzy sieci Web ASP.NET](#wfdatabinding)

    - [Korzystanie z powiązania modelu formularzy sieci Web](#wfmodelbinding)
    - [Korzystanie z kontrolek źródła danych formularzy sieci Web](#wfdsc)
    - [Korzystanie z kontrolek powiązanych z danymi formularzy sieci Web i wyrażeń powiązań danych](#wfdbc)
- [Praca z bazami danych SQL Server](#sqlserver)

    - [Praca z bazami danych SQL Server Express LocalDB](#sslocaldb)
    - [Praca z bazami danych SQL Server Express](#sse)
    - [Praca z systemem Windows Azure SQL Database](#ssdb)
    - [Wybieranie między SQL Server i Windows Azure SQL Database](#ssdbchoosing)
- [Praca z systemami zarządzania bazami danych NoSQL](#nosql)
- [Korzystanie z zapytań LINQ w aplikacjach ASP.NET](#linq)
- [Korzystanie z dynamicznego tworzenia szkieletów danych](#dd)
- [Zabezpieczanie dostępu do danych](#securing)
- [Optymalizowanie wydajności dostępu do danych](#optimizingdataaccess)
- [Wdrażanie bazy danych](#deploying)
- [Uzyskiwanie dostępu do danych za pomocą usługi sieci Web](#webservice)
- [Dodatkowe zasoby](#additional)

<a id="gettingstarted"></a>

## <a name="getting-started-with-data-access-in-aspnet"></a>Wprowadzenie z dostępem do danych w ASP.NET

- [Opcje magazynu danych (Tworzenie aplikacji w chmurze w rzeczywistych warunkach w systemie Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md). Rozdział książki elektronicznej dotyczącej tworzenia aplikacji w chmurze. Wprowadza bazy danych NoSQL jako alternatywę, że wielu deweloperów zaznajomionych z relacyjnymi bazami danych ma zapomina. Przedstawia wskazówki dotyczące tego, co należy wziąć pod uwagę w przypadku wybrania opcji relacyjne lub NoSQL lub wybrania konkretnej platformy.
- [ASP.NET — opcje dostępu do danych](https://msdn.microsoft.com/library/ms178359.aspx) (MSDN). Wprowadzenie do opcji dostępu do danych dla relacyjnych baz danych ASP.NET i wskazówki dotyczące wybierania platform i metod dostępu, które są odpowiednie dla danego scenariusza.
- [Relacyjna baza danych](http://en.wikipedia.org/wiki/Relational_database). Wikipedia). Jeśli nie pracujesz z relacyjnymi bazami danych, zapoznaj się z tą stroną, aby uzyskać wprowadzenie do terminologii i koncepcji relacyjnej bazy danych. Wprowadzenie do SQL Server w szczególności zobacz [Praca z bazami danych SQL Server](#sqlserver) w dalszej części tego tematu.

<a id="ef"></a>

## <a name="using-the-entity-framework"></a>Korzystanie z Entity Framework

- [Podejścia do programowania Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf) (MSDN). Wskazówki dotyczące wybierania Entity Framework podejścia deweloperskiego Database First, Model First lub Code First.

<a id="cf"></a>

### <a name="using-entity-framework-code-first"></a>Korzystanie z Entity Framework Code First

Następujące samouczki oferują przykładowe aplikacje do pobrania:

- [Wprowadzenie z dr 6 przy użyciu MVC 5](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Obejmuje szeroki zakres Entity Frameworkych scenariuszy Code First, w tym migracji i funkcji EF 6, takich jak odporność połączeń, przechwycenie poleceń i asynchroniczne. To jest zaktualizowana wersja programu [Dr 5/MVC 4](../mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Wcześniejsza seria zawiera samouczek dotyczący wzorców repozytorium i jednostek pracy, które nie są uwzględnione w nowej serii.
- [Wprowadzenie do ASP.NET MVC 5](../mvc/overview/getting-started/introduction/getting-started.md). Obejmuje węższy zakres Entity Frameworkych scenariuszy Code First, ale bardziej kompleksowe zadanie wprowadzenia funkcji MVC.
- [Powiązanie modelu i formularze sieci Web](https://go.microsoft.com/fwlink/?LinkId=286117). Używa Code First w aplikacji formularzy sieci Web.
- [Wprowadzenie z formularzami sieci Web ASP.NET 4,5](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Wprowadzenie do formularzy sieci Web z pewnym zakresem Code First. Używa powiązania modelu.
- [Sklep MVC Music](../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1.md). Używa Code First w aplikacji handlu elektronicznego MVC 3, która również implementuje członkostwo i autoryzację. Używany w tym miejscu system dostępu i wersja MVC (uwierzytelnianie i autoryzacja) ASP.NET są nieaktualne; Aby uzyskać bardziej aktualne informacje dotyczące członkostwa w usłudze ASP.NET, zobacz [https://asp.net/identity](https://asp.net/identity).

Inne zasoby:

- [Entity Framework Code First do istniejącej bazy danych](https://msdn.microsoft.com/data/jj200620). MSDN. Wideo i wskazówki, które pokazują, jak używać Code First z istniejącą bazą danych.
- [Centrum deweloperów danych — Entity Framework](https://msdn.microsoft.com/data/ef). MSDN. Aby uzyskać Przewodnik Entity Framework dokumentację utworzoną i obsługiwaną przez zespół Entity Framework, zobacz link [wprowadzenie](https://msdn.microsoft.com/data/ee712907) .

Zobacz również [książki dotyczące Entity Framework](#efbooks) i [dodatkowych zasobów Entity Framework](#otherefresources) w dalszej części tego tematu.

<a id="efcfmigrations"></a>

### <a name="using-entity-framework-code-first-migrations"></a>Używanie migracje Code First platformy Entity Framework

Większość samouczków Code First wymienionych powyżej dotyczy migracji. Zobacz również następujące zasoby.

- [ASP.NET wdrażanie w sieci Web przy użyciu programu Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). wieloczęściowa seria samouczków pokazująca, jak używać Migracje Code First do wdrażania bazy danych.
- [Wdróż aplikację Secure ASP.NET MVC 5 z członkostwem, uwierzytelnianiem OAuth i SQL Database w witrynie sieci Web systemu Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Microsoft Azure). Jak używać migracji, aby wdrażać dane dotyczące członkostwa i aplikacji na platformie Azure.
- [Omówienie wdrażania w sieci Web dla programu Visual Studio i ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment). Zapoznaj się z sekcją **Konfigurowanie wdrożenia bazy danych w programie Visual Studio** , aby dowiedzieć się, jak migracje Code First jest zintegrowana z funkcjami wdrażania w sieci Web programu Visual Studio.
- [Centrum deweloperów danych — migracje Code First](https://msdn.microsoft.com/data/jj591621) (MSDN). Dokumentacja migracji zespołu Entity Framework.
- [Migracja serii zrzut ekranu przedstawiający](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx). Blog Dr. Trzy filmy wideo dotyczące zaawansowanych tematów w Migracje Code First.
- [Migracje Code First z witrynami stron sieci Web ASP.NET](http://www.mikesdotnetting.com/Article/217/Code-First-Migrations-With-ASP.NET-Web-Pages-Sites). Blog Mikesdotnetting). Pokazuje, jak używać migracji Code First z witryną ASP.NET Web Pages, umieszczając kontekst danych w projekcie biblioteki klas programu Visual Studio.

<a id="efdbf"></a>

### <a name="using-entity-framework-database-first-or-model-first-the-ef-designer"></a>Korzystanie z Entity Framework Database First lub Model First (Projektant EF)

- [Wprowadzenie z Entity Framework 6 Database First przy użyciu MVC 5](../mvc/overview/getting-started/database-first-development/setting-up-database.md). Uruchom skrypt w Eksplorator serwera, aby utworzyć bazę danych, a następnie użyj projektanta Entity Framework, aby utworzyć model danych. Pokazuje sposób tworzenia prostych stron sieci Web CRUD oraz innych funkcji obsługi danych, które można wykonać po jednym z samouczków Code First, ponieważ wszystkie przepływy pracy EF używają tego samego interfejsu API DbContext.

Poniższe zasoby są starsze. Są one przydatne, jeśli chcesz użyć wersji 4,0 Entity Framework i chcesz użyć kontroli źródła danych dla powiązania danych w aplikacji formularzy sieci Web.

- [Wprowadzenie z Entity Framework 4,0](../web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). Pokazuje, jak używać kontrolki **EntityDataSource** .
- [Kontynuując Entity Framework](../web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)(pokazuje, jak używać kontrolki **ObjectDataSource** . Zawiera samouczek dotyczący obsługi współbieżności, samouczek dotyczący wydajności EF i samouczek dotyczący nowości w programie EF 4,0.

<a id="efrelateddata"></a>

### <a name="handling-related-data-in-entity-framework-lazy-loading-eager-loading-and-explicit-loading"></a>Obsługa powiązanych danych w Entity Framework (ładowanie z opóźnieniem, ładowanie eager i jawne ładowanie)

- [Odczytywanie powiązanych danych za pomocą Entity Framework w aplikacji ASP.NET MVC](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md). Code First, przykładowa aplikacja MVC. Przedstawione metody dotyczą także powiązania modelu formularzy sieci Web i przepływu pracy Database First.
- [Centrum deweloperów danych — ładowanie powiązanych jednostek](https://msdn.microsoft.com/data/jj574232) (MSDN). Dokumentacja zespołu Entity Frameworkowego dotycząca ładowania powiązanych danych.

<a id="optimizingef"></a>

### <a name="optimizing-entity-framework-performance"></a>Optymalizacja wydajności Entity Framework

- [Zaawansowane scenariusze Entity Framework dla aplikacji ASP.NET](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Pokazuje, jak wykonywać własne instrukcje SQL lub wywoływać własne procedury składowane, jak wyłączyć wykrywanie zmian i jak wyłączyć weryfikację podczas zapisywania zmian.
- [Zagadnienia dotyczące wydajności Entity Framework 5](https://msdn.microsoft.com/data/hh949853) (MSDN).
- [Zagadnienia dotyczące wydajności (Entity Framework)](https://msdn.microsoft.com/library/cc853327) (MSDN).
- [Maksymalizacja wydajności za pomocą Entity Framework w aplikacji sieci Web ASP.NET](../web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md). Dotyczy Entity Framework 4,0.
- Zobacz też [Optymalizacja dostępu do danych ASP.NET](#optimizingdataaccess) w dalszej części tego tematu.

<a id="efconcurrency"></a>

### <a name="handling-concurrency-in-an-entity-framework-application"></a>Obsługa współbieżności w aplikacji Entity Framework

- [Obsługa współbieżności przy użyciu Entity Framework w aplikacji ASP.NET MVC](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md). Code First, interfejs API DbContext, przy użyciu przykładowej aplikacji MVC.
- [Centrum deweloperów danych — optymistyczne wzorce współbieżności](https://msdn.microsoft.com/data/jj592904) (MSDN). Dokumentacja współbieżności zespołu Entity Framework.
- [Obsługa współbieżności przy użyciu Entity Framework w aplikacji sieci Web ASP.NET](../web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md). Dotyczy Entity Framework 4,0. Database First, interfejs API ObjectContext przy użyciu przykładowej aplikacji formularzy sieci Web.

<a id="efrepository"></a><a id="efbooks"></a>

### <a name="books-about-the-entity-framework"></a>Książki dotyczące Entity Framework

- [Programowanie Entity Framework: DbContext](http://shop.oreilly.com/product/0636920022237.do) przez Julie Lerman i Rowan Miller.
- [Programowanie Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) przez Julie Lerman i Rowan Miller.

Obie te książki są aktualne z bieżącymi zalecanymi technikami. Zapewniają one bardziej kompleksową i łatwą w obsłudze wprowadzenie do Entity Framework niż wszystkie dostępne w Internecie. Inna książka, [programowanie Entity Framework](http://shop.oreilly.com/product/9780596807252.do) przez Julie Lerman, jest większa i bardziej kompleksowa, ale nie jest już zalecanym sposobem korzystania z Entity Framework. Zobacz również listę książek zalecanych przez zespół Entity Framework w [Centrum deweloperów danych — książki](https://msdn.microsoft.com/data/aa937716) w witrynie MSDN.

<a id="otherefresources"></a>

### <a name="other-entity-framework-resources"></a>Inne zasoby Entity Framework

- [Blog zespołu Entity Framework (ADO.NET)](https://blogs.msdn.com/b/adonet/). Jeden z najlepszych zasobów dotyczących najnowszych informacji i anonsów nowych ulepszeń. W przypadku innych blogów związanych z EF zapoznaj się z tematem Blogroll w artykule wprowadzenie [do Entity Framework](https://msdn.microsoft.com/data/ee712907).
- [Magazyn MSDN](https://msdn.microsoft.com/magazine/default.aspx). Zobacz kolumnę **punkty danych** , która często zawiera tematy dotyczące Entity Framework.

<a id="wfdatabinding"></a>

## <a name="data-binding-in-aspnet-web-forms-applications"></a>Powiązanie danych w aplikacjach formularzy sieci Web ASP.NET

- [Opcje dostępu do danych formularzy sieci Web ASP.NET](https://msdn.microsoft.com/library/jj822927.aspx) (<a id="wfmodelbinding"></a>MSDN).

<a id="wfmodelbinding"></a>

### <a name="using-web-forms-model-binding"></a>Korzystanie z powiązania modelu formularzy sieci Web

- [Powiązanie modelu i formularze sieci Web](https://go.microsoft.com/fwlink/?LinkId=286117). Seria samouczków korzystających z Code First EF.
- [Powiązanie modelu formularzy sieci Web część 1: Wybieranie danych](https://weblogs.asp.net/scottgu/archive/2011/09/06/web-forms-model-binding-part-1-selecting-data-asp-net-vnext-series.aspx) (blog Scott Guthrie). W tych starszych wpisach w blogu właściwość o nazwie ItemType ma nazwę ModelType, ale w przeciwnym razie zawarte w niej informacje są prawidłowe.
- [Powiązanie modelu formularzy sieci Web część 2: filtrowanie danych](https://weblogs.asp.net/scottgu/archive/2011/09/12/web-forms-model-binding-part-2-filtering-data-asp-net-vnext-series.aspx) (blog Scott Guthrie).
- [Powiązanie modelu formularzy sieci Web część 3: aktualizowanie i walidacja](https://weblogs.asp.net/scottgu/archive/2011/10/30/web-forms-model-binding-part-3-updating-and-validation-asp-net-4-5-series.aspx) (blog Scott Guthrie).
- [Powiązanie modelu formularzy sieci Web w programie ASP.NET 4,5](../web-forms/videos/aspnet-web-forms-vnext/aspnet-45-web-forms-model-binding.md). (wideo).
- [Powiązanie modelu część 1 — Wybieranie danych](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-1-selecting-data.md) (wideo).
- [Powiązanie modelu część 2 — filtrowanie](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-2-filtering.md) (wideo).
- [Wprowadzenie z formularzami sieci Web ASP.NET 4,5 — wyświetla elementy danych i szczegóły](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).

<a id="wfdsc"></a>

### <a name="using-web-forms-data-source-controls"></a>Korzystanie z kontrolek źródła danych formularzy sieci Web

- [Formanty serwera sieci Web źródła danych](https://msdn.microsoft.com/library/ms247258.aspx) (MSDN).
- [Ogłoszenie dostawcy danych dynamicznych i kontrolki EntityDataSource dla Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/02/28/announcing-the-release-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx) (blog Microsoft Web Development).

<a id="wfdbc"></a>

### <a name="using-web-forms-data-bound-controls-and-data-binding-expressions"></a>Korzystanie z kontrolek powiązanych z danymi formularzy sieci Web i wyrażeń powiązań danych

- [Powiązanie modelu i formularze sieci Web](https://go.microsoft.com/fwlink/?LinkId=286117). Seria samouczków korzystających z Code First EF.
- [Wprowadzenie z formularzami sieci Web ASP.NET 4,5 — wyświetla elementy danych i szczegóły](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).
- [Kontrolki danych o jednoznacznie określonym typie](https://weblogs.asp.net/scottgu/archive/2011/09/02/strongly-typed-data-controls-asp-net-vnext-series.aspx) (blog Scott Guthrie).
- [Kontrolki danych silnie wpisane](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (wideo).
- [ASP.NET 4,5 Web Forms (wideo) o silnych typach danych](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) .
- [Formanty serwera sieci Web powiązane z danymi](https://msdn.microsoft.com/library/ms228214.aspx) (MSDN).
- [Wyrażenia powiązań danych — omówienie](https://msdn.microsoft.com/library/ms178366.aspx) (MSDN). Ta **Strona obejmuje tylko** ocenę i **powiązanie**; nie została zaktualizowana w celu uwzględnienia **elementu** i **binditem**.

<a id="sqlserver"></a>

## <a name="working-with-sql-server-databases"></a>Praca z bazami danych SQL Server

- [Funkcje bazy danych SQL Server](https://msdn.microsoft.com/library/hh230827.aspx) (MSDN). Aby zapoznać się z ogólnym wprowadzeniem do szerokiej gamy tematów SQL Server, zobacz wpisy w obszarze spisu treści.
- [Wersje SQL Server](https://msdn.microsoft.com/library/ms178359.aspx#sqlserver) (MSDN). Podsumowanie dostępnych wersji SQL Server z linkami do dodatkowych informacji o każdej z nich.)
- [SQL Server parametry połączenia dla aplikacji sieci Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Używanie SQL Server Compact dla aplikacji sieci Web ASP.NET](https://msdn.microsoft.com/library/ms247257.aspx) (MSDN).
- [Microsoft SQL Server: przykłady produktu bazy danych](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Przykładowe bazy danych AdventureWorks.
- [Instalowanie przykładowych baz danych](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Oprócz metod przedstawionych w tym miejscu można również pobrać jeden z przykładowych plików. mdf do folderu danych\_aplikacji projektu sieci Web, przekonwertować bazę danych na LocalDB i utworzyć parametry połączenia LocalDB. Aby uzyskać informacje o tym, jak to zrobić, zobacz [How to: Upgrade to LocalDB](https://msdn.microsoft.com/library/hh873188.aspx).

Zobacz również następujące sekcje dotyczące pracy z SQL Server Express i LocalDB i wybierania między SQL Server i SQL Database.

<a id="sslocaldb"></a>

### <a name="working-with-sql-server-express-localdb-databases"></a>Praca z bazami danych SQL Server Express LocalDB

- [SQL Server Express 2012 LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (MSDN). Oficjalna witryna MSDN LocalDB.
- [SQL Server parametry połączenia dla aplikacji sieci Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Instrukcje: uaktualnianie do LocalDB](https://msdn.microsoft.com/library/hh873188.aspx) (MSDN). Jak migrować plik MDF ze starszej wersji SQL Server Express do LocalDB. Konieczne jest również przechodzenie przez ten proces w przypadku pobrania jednej z [przykładowych baz danych SQL Server 2012](https://go.microsoft.com/fwlink/?linkid=117483).
- [Wprowadzenie do LocalDB — udoskonalonego blogu SQL Express](https://go.microsoft.com/fwlink/?LinkId=234375) (SQL Server Express). Ma więcej informacji na temat tego, dlaczego LocalDB został utworzony niż jest zawarty w witrynie MSDN.
- [LocalDB.: gdzie jest moja baza danych?](https://go.microsoft.com/fwlink/?LinkId=234376) (Blog SQL Server Express). Informacje o tym, gdzie są tworzone pliki bazy danych LocalDB.
- [Korzystanie z LocalDB z pełnymi usługami IIS — część 1: profil użytkownika](https://blogs.msdn.com/b/sqlexpress/archive/2011/12/09/using-localdb-with-full-iis-part-1-user-profile.aspx) (blog SQL Server Express). LocalDB nie jest zaprojektowana do pracy z usługami IIS. W tej serii wpisów w blogu wyjaśniono problemy i niektóre obejścia tego problemu.

<a id="sse"></a>

### <a name="working-with-sql-server-express-databases"></a>Praca z bazami danych SQL Server Express

- [SQL Server parametry połączenia dla aplikacji sieci Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN). Jeśli używasz ustawienia parametrów połączenia AttachDBFileName z SQL Server Express, zobacz sekcję szczególnie wystąpienie użytkownika na tej stronie.
- [Jak przejąć własność lokalnego SQL Server Express 2008](https://blogs.msdn.com/b/sqlexpress/archive/2010/02/23/how-to-take-ownership-of-your-local-sql-server-2008-express.aspx) (SQL Server Express blogu). Typowy problem nie może współdziałać z bazami danych SQL Server Express, ponieważ nie jesteś administratorem wystąpienia SQL Server Express. Domyślnie tylko osoba, która zainstalowała SQL Server Express jest administratorem. W tym blogu wyjaśniono, jak sprawić, aby jesteś administratorem SQL Server Express, jeśli jesteś administratorem na komputerze.
- [Czy moja aplikacja sieci Web ASP.NET korzysta z bazy danych SQL Server Express w środowisku produkcyjnym?](https://msdn.microsoft.com/library/jj653753.aspx#sql_express_in_production) (MSDN).

<a id="ssdb"></a>

### <a name="working-with-windows-azure-sql-database"></a>Praca z systemem Windows Azure SQL Database

- [Wdróż aplikację Secure ASP.NET MVC z członkostwem, uwierzytelnianiem OAuth i SQL Database w witrynie sieci Web systemu Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data) (witryna Microsoft Azure).
- [Bazy danych SQL](https://docs.microsoft.com/azure/sql-database/) (Microsoft Azure Site). Samouczki rozpoczynające pracę i przewodniki.
- [Azure SQL Database systemu Windows](https://msdn.microsoft.com/library/windowsazure/ee336279.aspx) (MSDN). Węzeł najwyższego poziomu spisu treści dla SQL Database w witrynie MSDN.
- [Indeks artykułów w witrynie TechNet systemu Windows Azure SQL Database](https://social.technet.microsoft.com/wiki/contents/articles/2267.windows-azure-sql-database-technet-wiki-articles-index-en-us.aspx) . (witryna Microsoft TechNet).
- [Blok aplikacji do obsługi błędów przejściowych](https://msdn.microsoft.com/library/hh680934(v=PandP.50).aspx). Struktura, która umożliwia obsługę przejściowych błędów sieci i błędy połączeń wynikających z ograniczenia. Dostępne w pakiecie NuGet: [Biblioteka Enterprise 5,0 — blok aplikacji obsługi błędów przejściowych](http://nuget.org/packages/EnterpriseLibrary.WindowsAzure.TransientFaultHandling).
- [Wprowadzenie z SQL Database i Entity Framework](https://msdn.microsoft.com/data/jj556244) (MSDN).
- [Windows Azure Training Kit](https://www.microsoft.com/download/details.aspx?id=8396) (centrum pobierania Microsoft). Obejmuje laboratorium praktyczne dla SQL Database.
- [Forum społeczności systemu Windows Azure SQL Database](https://social.msdn.microsoft.com/Forums/ssdsgetstarted/threads).
- [Przejście do Azure SQL Database systemu Windows](https://msdn.microsoft.com/library/ff803375.aspx) (MSDN). Jeden rozdział kompleksowego scenariusza przez zespół ds. wzorców i praktyk firmy Microsoft. Zawarto informacje na temat tego, dlaczego warto przeprowadzić migrację i jak przeprowadzić migrację z SQL Server do SQL Database.
- [Migrowanie baz danych SQL Server do Azure SQL Database systemu Windows](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN).
- [Kreator migracji SQL Database](http://sqlazuremw.codeplex.com/). Narzędzie open source do migrowania baz danych do i z SQL Database.

<a id="ssdbchoosing"></a>

### <a name="choosing-between-sql-server-and-windows-azure-sql-database"></a>Wybieranie między SQL Server i Windows Azure SQL Database

- [Porównaj SQL Server z Azure SQL Database Windows](https://social.technet.microsoft.com/wiki/contents/articles/996.compare-sql-server-with-windows-azure-sql-database-en-us.aspx) (witryna Microsoft TechNet).
- [Migracja danych do systemu Windows Azure SQL Database: narzędzia i techniki](https://msdn.microsoft.com/library/windowsazure/hh694043.aspx) (MSDN). Zawiera sekcje, które porównują SQL Server do SQL Database i udostępniają wskazówki dotyczące migracji z SQL Server do SQL Database.
- [Windows Azure SQL Database Delivery Guide](https://social.technet.microsoft.com/wiki/contents/articles/3398.windows-azure-sql-database-delivery-guide-en-us.aspx) (witryna Microsoft TechNet).
- [Ograniczenia funkcji SQL Server (Windows Azure SQL Database)](https://msdn.microsoft.com/library/windowsazure/ff394115.aspx) (MSDN).
- [Windows Azure Table Storage i windows Azure SQL Database — porównane i rozróżnienia](https://msdn.microsoft.com/library/jj553018.aspx) (MSDN). W przypadku aplikacji wdrażanej na platformie Microsoft Azure magazyn tabel systemu Windows Azure może być alternatywą dla systemu Windows Azure SQL Database. Ten temat ułatwia podejmowanie decyzji między tymi alternatywami.
- [Azure SQL Database systemu Windows](https://msdn.microsoft.com/library/windowsazure/ee336279) (MSDN).
- [Wytyczne i ograniczenia (Windows Azure SQL Database)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx)

<a id="nosql"></a>

## <a name="working-with-nosql-database-management-systems"></a>Praca z systemami zarządzania bazami danych NoSQL

- [Data Services systemu Windows Azure](https://www.windowsazure.com/develop/net/data/) (witryna Microsoft Azure). Zobacz temat [Przewodnik po funkcjach usługi Table Service](https://docs.microsoft.com/azure/) oraz sekcja **Big Data** na stronie.
- [ASP.NET wielowarstwową aplikację korzystającą z tabel, kolejek i obiektów blob magazynu](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (Microsoft Azure Site). Kompleksowy samouczek z przykładową aplikacją do pobrania, która używa tabel usługi Microsoft Azure Storage NoSQL.

<a id="linq"></a>

## <a name="using-linq-queries-in-aspnet-applications"></a>Korzystanie z zapytań LINQ w aplikacjach ASP.NET

- [ASP.NET — opcje dostępu do danych](https://msdn.microsoft.com/library/ms178359.aspx#linq) (MSDN). Zawiera wprowadzenie do programu LINQ.
- [Wideo z szkoleniami LINQ](http://www.misfitgeek.com/windows-client-linq-training-videos-20/) (blog Jan Stagner).
- [Wątek Forum ASP.NET z linkami do dynamicznych zasobów LINQ](https://forums.asp.net/p/1961037/5601994.aspx?Please+suggest+good+books+so+that+one+can+write+and+understand+dynamic+linq).

<a id="dd"></a>

## <a name="using-dynamic-data-scaffolding"></a>Korzystanie z dynamicznego tworzenia szkieletów danych

- [Szablony projektów danych dynamicznych](https://msdn.microsoft.com/library/jj822927.aspx#dynamicdata) (MSDN). Wskazówki dotyczące korzystania z projektów danych dynamicznych.
- [ASP.NET dane dynamiczne](https://msdn.microsoft.com/library/ee845452.aspx) (MSDN).

<a id="securing"></a>

## <a name="securing-data-access"></a>Zabezpieczanie dostępu do danych

- [Zabezpieczanie dostępu do danych w usłudze ASP.NET](https://msdn.microsoft.com/library/ms178375.aspx) (MSDN).
- [Zagadnienia dotyczące zabezpieczeń (Entity Framework)](https://msdn.microsoft.com/library/cc716760.aspx) (MSDN).
- [Instrukcje: Zabezpieczanie parametrów połączenia w przypadku korzystania z formantów źródła danych](https://msdn.microsoft.com/library/ms178372.aspx) (MSDN).

<a id="optimizingdataaccess"></a>

## <a name="optimizing-data-access-performance"></a>Optymalizowanie wydajności dostępu do danych

- [Przegląd wydajności ASP.NET](https://msdn.microsoft.com/library/cc668225.aspx) (MSDN).
- [Buforowanie ASP.NET](https://msdn.microsoft.com/library/xsbfdd8c.aspx) (MSDN).
- [Poprawa wydajności ASP.NET](https://msdn.microsoft.com/library/ff647787) (MSDN). W górnej części tej strony znajduje się ostrzeżenie "wycofana zawartość", ale większość informacji jest nadal istotna i nie ma żadnych podobnych zaktualizowanych zasobów.
- [Poprawianie wydajności SQL Server](https://msdn.microsoft.com/library/ff647793) (MSDN). Ten sam komentarz co poprzedni link.

Zobacz też [Optymalizacja wydajności Entity Framework](#optimizingef) wcześniej w tym temacie.

<a id="deploying"></a>

## <a name="deploying-a-database"></a>Wdrażanie bazy danych

- [ASP.NET Web Deployment — zalecane zasoby](aspnet-web-deployment-content-map.md) (MSDN).

<a id="webservice"></a>

## <a name="accessing-data-through-a-web-service"></a>Uzyskiwanie dostępu do danych za pomocą usługi sieci Web

- [Uzyskiwanie dostępu do danych za pomocą usługi sieci Web](https://msdn.microsoft.com/library/ms178359.aspx#webservice) (MSDN). Wskazówki dotyczące korzystania z interfejsu API sieci Web i programu WCF.
- [Wprowadzenie z interfejsem API sieci Web ASP.NET](../web-api/index.md).
- [Usługi danych programu WCF](https://msdn.microsoft.com/data/bb931106) (MSDN).

<a id="additional"></a>

## <a name="additional-resources"></a>Dodatkowe materiały

- [ASP.NET — często zadawane pytania dotyczące dostępu do danych](https://msdn.microsoft.com/library/jj653753.aspx) (MSDN).
- [Samouczki formularzy sieci Web ASP.NET — dane](../web-forms/overview/data-access/index.md). Większość z tych samouczków jest stosunkowo stara; Upewnij się, że [ASP.NET opcje dostępu do danych](https://msdn.microsoft.com/library/ms178359.aspx) i [Magazyn danych (w przypadku tworzenia rzeczywistych aplikacji w chmurze w systemie Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md) , tak aby nie było zbyt daleko do metody dostępu do danych, która nie jest odpowiednia dla Twojego scenariusza.
- [Mapa zawartości ASP.NET MVC](../mvc/overview/getting-started/recommended-resources-for-mvc.md).
- [Samouczki dotyczące ASP.NET stron sieci Web — dane](../web-pages/overview/data/index.md).
- [Uzyskiwanie dostępu do danych w programie Visual Studio](https://msdn.microsoft.com/library/wzabh8c4.aspx) (MSDN). Zawiera listę linków podobnych do tej mapy zawartości, ale z fokusem w programie Visual Studio, a nie ASP.NET.

---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: Tworzenie modelu danych Entity Framework dla aplikacji ASP.NET MVC (1 z 10) | Microsoft Docs
author: tdykstra
description: Dostępna jest nowsza wersja tej serii samouczków, dla Visual Studio 2013, Entity Framework 6 i MVC 5. Przykładowa aplikacja internetowa firmy Contoso University de...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 4ba029b6-ee7c-4e45-a0e7-b703c37e5d9a
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 8ee5aa22b6b2329b01d41437f30508e28a2288b2
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74595967"
---
# <a name="creating-an-entity-framework-data-model-for-an-aspnet-mvc-application-1-of-10"></a>Tworzenie modelu danych Entity Framework dla aplikacji ASP.NET MVC (1 z 10)

Autor [Dykstra](https://github.com/tdykstra)

[Pobierz ukończony projekt](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> > [!NOTE] 
> > 
> > Dostępna jest [nowsza wersja tej serii samouczków](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) , dla Visual Studio 2013, Entity Framework 6 i MVC 5.
> 
> 
> Przykładowa aplikacja internetowa Contoso University pokazuje, jak tworzyć aplikacje ASP.NET MVC 4 przy użyciu Entity Framework 5 i Visual Studio 2012. Przykładowa aplikacja jest witryną internetową fikcyjnej firmy Contoso University. Obejmuje to funkcje, takie jak przyjmowanie studentów, tworzenie kursu i przydziały instruktora. W tej serii samouczków wyjaśniono, jak skompilować przykładową aplikację firmy Contoso University. Możesz [pobrać ukończoną aplikację](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8).
> 
> ## <a name="code-first"></a>Code First
> 
> Istnieją trzy sposoby pracy z danymi w Entity Framework: *Database First*, *model First*i *Code First*. Ten samouczek jest przeznaczony dla Code First. Aby uzyskać informacje o różnicach między tymi przepływami pracy i wskazówkami dotyczącymi sposobu wybierania najlepszego z nich dla danego scenariusza, zobacz [Entity Frameworke przepływy pracy projektowania](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="mvc"></a>MVC
> 
> Przykładowa aplikacja jest oparta na [ASP.NET MVC](../../../index.md). Jeśli wolisz pracować z modelem ASP.NET Web Forms, zapoznaj się z serią samouczków [i formularzy sieci Web](../../../../web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data.md) oraz [mapę zawartości dostęp do danych ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).
> 
> ## <a name="software-versions"></a>Wersje oprogramowania
> 
> | **Pokazane w samouczku** | **Współpracuje również z** |
> | --- | --- |
> | Windows 8 | Windows 7 |
> | Visual Studio 2012 | Visual Studio 2012 Express for Web. Jest to automatycznie instalowane przez zestaw SDK systemu Windows Azure, jeśli nie masz jeszcze programu VS 2012 lub VS 2012 Express for Web. Visual Studio 2013 powinna być zadziałała, ale samouczek nie został przetestowany z nim, a niektóre opcje menu i okna dialogowe są różne. Do wdrożenia systemu Windows Azure jest wymagany [wersja VS 2013 zestawu Windows Azure SDK](https://go.microsoft.com/fwlink/p/?linkid=323510) . |
> | .NET 4.5 | Większość widocznych funkcji będzie działała w środowisku .NET 4, ale niektóre z nich nie będą. Na przykład obsługa wyliczenia w EF wymaga programu .NET 4,5. |
> | Entity Framework 5 |  |
> | [Zestaw SDK systemu Windows Azure 2,1](https://go.microsoft.com/fwlink/p/?linkid=323511) | W przypadku pominięcia kroków wdrażania systemu Windows Azure nie jest potrzebny zestaw SDK. Po wydaniu nowej wersji zestawu SDK link zainstaluje nowszą wersję. W takim przypadku może być konieczne dostosowanie niektórych instrukcji do nowego interfejsu użytkownika i funkcji. |
> 
> ## <a name="questions"></a>Masz
> 
> Jeśli masz pytania, które nie są bezpośrednio związane z samouczkiem, możesz je ogłosić na [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), [forum Entity Framework i LINQ to Entities](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/)lub [StackOverflow.com](http://stackoverflow.com/).
> 
> ## <a name="acknowledgments"></a>Potwierdzeń
> 
> Zapoznaj się z ostatnim samouczkiem w serii dla [potwierdzeń i zanotuj informacje o języku VB](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#acknowledgments).
> 
> ## <a name="original-version-of-the-tutorial"></a>Oryginalna wersja samouczka
> 
> Oryginalna wersja samouczka jest dostępna w [książce elektronicznej dr 4,1/MVC 3](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC).

## <a name="the-contoso-university-web-application"></a>Aplikacja sieci Web firmy Contoso University

Aplikacja, którą tworzysz w tych samouczkach, jest prostą witryną sieci Web.

Użytkownicy mogą wyświetlać i aktualizować informacje dotyczące uczniów, kursów i instruktorów. Poniżej przedstawiono kilka ekranów, które zostaną utworzone.

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

Styl interfejsu użytkownika tej witryny został zamknięty w pobliżu elementów generowanych przez wbudowane szablony, dzięki czemu samouczek może skupić się głównie na sposobach używania Entity Framework.

## <a name="prerequisites"></a>Wymagania wstępne

Instrukcje i zrzuty ekranu w tym samouczku założono, że używasz [programu Visual studio 2012](https://www.microsoft.com/visualstudio/eng/downloads) lub [Visual Studio 2012 Express for Web](https://go.microsoft.com/fwlink/?LinkID=275131), wraz z najnowszymi aktualizacjami i zestawem SDK platformy Azure dla platformy .NET zainstalowanym od lipca 2013. Wszystko to można uzyskać za pomocą następującego linku:

[Zestaw Azure SDK dla platformy .NET (Visual Studio 2012)](https://go.microsoft.com/fwlink/?LinkId=254364)

Jeśli masz zainstalowany program Visual Studio, powyższe łącze zainstaluje wszystkie brakujące składniki. Jeśli nie masz programu Visual Studio, link zainstaluje program Visual Studio 2012 Express for Web. Można użyć Visual Studio 2013, ale niektóre z wymaganych procedur i ekranów będą się różnić.

## <a name="create-an-mvc-web-application"></a>Tworzenie aplikacji sieci Web MVC

Otwórz program Visual Studio i Utwórz nowy C# projekt o nazwie "ContosoUniversity" przy użyciu szablonu **aplikacji sieci Web ASP.NET MVC 4** . Upewnij się, że docelowa **.NET Framework 4,5** (będziesz używać [Właściwości`enum`](https://msdn.microsoft.com/data/hh859576.aspx), które wymagają programu .NET 4,5).

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

W oknie dialogowym **Nowy projekt ASP.NET MVC 4** wybierz szablon **aplikacja internetowa** .

Pozostaw wybrany aparat widoku **Razor** i Pozostaw zaznaczenie pola wyboru **Utwórz projekt testu jednostkowego** .

Kliknij przycisk **OK**.

![Project_template_options](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

## <a name="set-up-the-site-style"></a>Konfigurowanie stylu witryny

Kilka prostych zmian spowoduje skonfigurowanie menu witryny, układu i strony głównej.

Otwórz plik *Views\Shared\\_Layout. cshtml*i Zastąp zawartość pliku następującym kodem. Zmiany są wyróżnione.

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=5,15,25-28,43)]

Ten kod wprowadza następujące zmiany:

- Zastępuje wystąpienia szablonu "My ASP.NET Application MVC" i "logo tutaj" "szkołą firmy Contoso".
- Dodaje kilka linków akcji, które zostaną użyte w dalszej części samouczka.

W *Views\Home\Index.cshtml*Zastąp zawartość pliku następującym kodem, aby wyeliminować akapity szablonu dotyczące ASP.NET i MVC:

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

W *Controllers\HomeController.cs*zmień wartość `ViewBag.Message` w metodzie `Index` akcji na "Witamy w firmie Contoso University!", jak pokazano w następującym przykładzie:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=3)]

Naciśnij klawisze CTRL + F5, aby uruchomić lokację. Zostanie wyświetlona strona główna z menu głównym.

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

## <a name="create-the-data-model"></a>Tworzenie modelu danych

Następnie utworzysz klasy jednostek dla aplikacji firmy Contoso University. Zaczniesz od następujących trzech jednostek:

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

Istnieje relacja jeden do wielu między jednostkami `Student` i `Enrollment` i istnieje relacja jeden do wielu między `Course` i `Enrollment` jednostek. Innymi słowy, student może być zarejestrowany w dowolnej liczbie kursów, a kurs może mieć dowolną liczbę uczniów zarejestrowanych w nim.

W poniższych sekcjach utworzysz klasę dla każdej z tych jednostek.

> [!NOTE]
> Jeśli spróbujesz skompilować projekt przed ukończeniem tworzenia wszystkich tych klas jednostek, uzyskasz błędy kompilatora.

### <a name="the-student-entity"></a>Jednostka ucznia

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

W folderze *modele* Utwórz *student.cs* i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

Właściwość `StudentID` stanie się kolumną klucza podstawowego tabeli bazy danych, która odnosi się do tej klasy. Domyślnie Entity Framework interpretuje właściwość o nazwie `ID` lub *classname* `ID` jako klucz podstawowy.

Właściwość `Enrollments` jest *właściwością nawigacji*. Właściwości nawigacji zawierają inne jednostki, które są powiązane z tą jednostką. W takim przypadku Właściwość `Enrollments` jednostki `Student` będzie zawierać wszystkie jednostki `Enrollment`, które są powiązane z tą jednostką `Student`. Innymi słowy, jeśli dany wiersz `Student` w bazie danych ma dwa powiązane `Enrollment` wiersze (wiersze, które zawierają wartość klucza podstawowego tego ucznia w `StudentID` kolumnie klucza obcego), właściwość nawigacji `Enrollments` jednostki `Student` będzie zawierać te dwie jednostki `Enrollment`.

Właściwości nawigacji są zwykle zdefiniowane jako `virtual`, dzięki czemu mogą korzystać z niektórych funkcji Entity Framework, takich jak *ładowanie z opóźnieniem*. (Ładowanie z opóźnieniem zostanie omówione później, w samouczku [odczytywanie powiązanych danych](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) w dalszej części tej serii).

Jeśli właściwość nawigacji może zawierać wiele jednostek (tak jak w przypadku relacji "wiele do wielu" lub "jeden do wielu"), jej typem musi być lista, w której można dodawać, usuwać i aktualizować wpisy, takie jak `ICollection`.

### <a name="the-enrollment-entity"></a>Jednostka rejestracji

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

W folderze *modele* Utwórz *Enrollment.cs* i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

Właściwość klasy jest [wyliczeniem](https://msdn.microsoft.com/data/hh859576.aspx). Znak zapytania po deklaracji typu `Grade` wskazuje, że właściwość `Grade` ma [wartość null](https://msdn.microsoft.com/library/2cf62fcy.aspx). Klasa o wartości null różni się od klasy zerowej — wartość null oznacza, że Klasa nie jest znana lub nie została jeszcze przypisana.

Właściwość `StudentID` jest kluczem obcym, a odpowiednia właściwość nawigacji jest `Student`. Jednostka `Enrollment` jest skojarzona z jedną jednostką `Student`, więc właściwość może zawierać tylko jedną jednostkę `Student` (w przeciwieństwie do `Student.Enrollments`j właściwości nawigacji, która może być przechowywana w wielu jednostkach `Enrollment`).

Właściwość `CourseID` jest kluczem obcym, a odpowiednia właściwość nawigacji jest `Course`. Jednostka `Enrollment` jest skojarzona z jedną jednostką `Course`.

### <a name="the-course-entity"></a>Jednostka kursu

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

W folderze *modele* Utwórz *Course.cs*, zastępując istniejący kod następującym kodem:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

Właściwość `Enrollments` jest właściwością nawigacji. Jednostka `Course` może być powiązana z dowolną liczbą `Enrollment` jednostek.

Dowiesz się więcej na temat [[DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx)([DatabaseGeneratedOption](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.95).aspx). None)] w następnym samouczku. Zasadniczo ten atrybut umożliwia wprowadzenie klucza podstawowego dla kursu, a nie jego wygenerowanie.

## <a name="create-the-database-context"></a>Tworzenie kontekstu bazy danych

Klasa główna, która koordynuje funkcje Entity Framework dla danego modelu danych, jest klasą *kontekstu bazy danych* . Tę klasę można utworzyć, pobierając ją z klasy [System. Data. Entity. DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) . W kodzie możesz określić, które jednostki zostaną uwzględnione w modelu danych. Można również dostosować pewne zachowanie Entity Framework. W tym projekcie Klasa ma nazwę `SchoolContext`.

Utwórz folder o nazwie *dal* (dla warstwy dostępu do danych). W tym folderze Utwórz nowy plik klasy o nazwie *SchoolContext.cs*i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs)]

Ten kod tworzy właściwość [nieogólnymi](https://msdn.microsoft.com/library/system.data.entity.dbset(v=VS.103).aspx) dla każdego zestawu jednostek. W Entity Framework terminologii *zestaw jednostek* zwykle odpowiada tabeli bazy danych, a *Jednostka* odpowiada wierszowi w tabeli.

Instrukcja `modelBuilder.Conventions.Remove` w metodzie [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) zapobiega dokonywaniu pluralizmu nazw tabel. Jeśli nie zostało to zrobione, wygenerowane tabele byłyby nazwane `Students`, `Courses`i `Enrollments`. Zamiast tego nazwy tabel będą `Student`, `Course`i `Enrollment`. Deweloperzy nie zgadzają się, czy nazwy tabel powinny być w liczbie mnogiej. W tym samouczku jest używana forma Jednoargumentowa, ale ważnym punktem jest to, że można wybrać dowolną formę, wybierając lub pomijając ten wiersz kodu.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx) to uproszczona wersja aparatu bazy danych SQL Server Express uruchamiana na żądanie i uruchamiana w trybie użytkownika. LocalDB działa w specjalnym trybie wykonywania SQL Server Express, który umożliwia współpracę z bazami danych jako plikami *MDF* . Zazwyczaj pliki bazy danych LocalDB są przechowywane w folderze *danych\_aplikacji* projektu sieci Web. Funkcja wystąpienia użytkownika w SQL Server Express umożliwia również pracy z plikami *. mdf* , ale funkcja wystąpienia użytkownika jest przestarzała; w związku z tym LocalDB jest zalecane do pracy z plikami *. mdf* .

Zwykle SQL Server Express nie jest używany dla produkcyjnych aplikacji sieci Web. LocalDB w szczególności nie są zalecane do użycia w środowisku produkcyjnym z aplikacją sieci Web, ponieważ nie jest ona przeznaczona do pracy z usługami IIS.

W programie Visual Studio 2012 i nowszych wersjach LocalDB jest instalowany domyślnie z programem Visual Studio. W programie Visual Studio 2010 i starszych wersjach SQL Server Express (bez LocalDB) jest instalowany domyślnie z programem Visual Studio; musisz zainstalować go ręcznie, jeśli używasz programu Visual Studio 2010.

W tym samouczku przedstawiono współpracę z usługą LocalDB, dzięki czemu baza danych będzie mogła być przechowywana w folderze *danych\_w aplikacji* jako plik *. mdf* . Otwórz główny plik *Web. config* i Dodaj nowe parametry połączenia do kolekcji `connectionStrings`, jak pokazano w poniższym przykładzie. (Pamiętaj, aby zaktualizować plik *Web. config* w folderze głównym projektu. Istnieje również plik *Web. config* znajdujący się w podfolderze *widoki* , które nie są potrzebne do aktualizacji.

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.xml)]

Domyślnie Entity Framework szuka parametrów połączenia o nazwie takiej samej jak Klasa `DbContext` (`SchoolContext` dla tego projektu). Dodane parametry połączenia określają LocalDB bazę danych o nazwie *ContosoUniversity. mdf* znajdującą się w folderze *danych\_aplikacji* . Aby uzyskać więcej informacji, zobacz [SQL Server parametry połączenia dla aplikacji sieci Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).

Nie trzeba określać parametrów połączenia. Jeśli nie podasz parametrów połączenia, Entity Framework utworzy je dla Ciebie; Jednak baza danych może nie znajdować się w folderze *danych\_* aplikacji. Aby uzyskać informacje o tym, gdzie zostanie utworzona baza danych, zobacz [Code First do nowej bazy danych](https://msdn.microsoft.com/data/jj193542).

Kolekcja `connectionStrings` ma również parametry połączenia o nazwie `DefaultConnection`, które są używane dla bazy danych członkostwa. Nie będziesz korzystać z bazy danych członkostwa w tym samouczku. Jedyną różnicą między dwoma ciągami połączenia jest nazwa bazy danych i wartość atrybutu nazwy.

## <a name="set-up-and-execute-a-code-first-migration"></a>Konfigurowanie i wykonywanie migracji Code First

Po pierwszym rozpoczęciu opracowywania aplikacji model danych zmienia się często, a przy każdym zmianie modelu nie jest on synchronizowany z bazą danych. Można skonfigurować Entity Framework, aby automatycznie usuwać i ponownie tworzyć bazę danych przy każdej zmianie modelu danych. Nie jest to problem wczesny podczas opracowywania, ponieważ dane testowe można łatwo utworzyć ponownie, ale po wdrożeniu w środowisku produkcyjnym zwykle trzeba zaktualizować schemat bazy danych bez porzucania bazy danych. Funkcja migracji umożliwia Code First aktualizowanie bazy danych bez porzucania i ponownego tworzenia. Na wczesnym etapie tworzenia nowego projektu można użyć [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=vs.103).aspx) , aby porzucić, odtworzyć i ponownie utworzyć bazę danych za każdym razem, gdy zmienia się model. Po przygotowaniu do wdrożenia aplikacji można przeprowadzić konwersję na podejście do migracji. Na potrzeby tego samouczka będziesz używać tylko migracji. Aby uzyskać więcej informacji, zobacz [migracje Code First](https://msdn.microsoft.com/data/jj591621) i [migracji zrzut ekranu przedstawiający Series](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx).

### <a name="enable-code-first-migrations"></a>Włącz Migracje Code First

1. W menu **Narzędzia** kliknij pozycję **Menedżer pakietów NuGet** , a następnie **konsolę Menedżera pakietów**.

    ![Selecting_Package_Manager_Console](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)
2. W wierszu `PM>` wprowadź następujące polecenie:

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.ps1)]

    ![Enable-migrations — polecenie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

    To polecenie powoduje utworzenie folderu *migracji* w projekcie ContosoUniversity i umieszczenie w nim pliku *Configuration.cs* , który można edytować w celu skonfigurowania migracji.

    ![Folder migracji](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)

    Klasa `Configuration` obejmuje metodę `Seed`, która jest wywoływana, gdy baza danych jest tworzona, a za każdym razem po zmianie modelu danych.

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

    Celem tej metody `Seed` jest umożliwienie wstawiania danych testowych do bazy danych po Code First utworzeniu lub zaktualizowaniu.

### <a name="set-up-the-seed-method"></a>Konfigurowanie metody inicjatora

Metoda [inicjatora](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) jest uruchamiana, gdy migracje Code First tworzy bazę danych i za każdym razem aktualizuje bazę danych do najnowszej migracji. Celem metody inicjatora jest umożliwienie wstawiania danych do tabel, zanim aplikacja uzyskuje dostęp do bazy danych po raz pierwszy.

We wcześniejszych wersjach Code First przed udostępnieniem migracji było ono wspólne dla `Seed` metod wstawiania danych testowych, ponieważ w przypadku każdej zmiany modelu podczas programowania baza danych musiała zostać całkowicie usunięta i ponownie utworzona od podstaw. Przy Migracje Code First dane testowe są zachowywane po zmianie bazy danych, więc w tym przypadku dane testowe w metodzie [inicjatora](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) zwykle nie są konieczne. W rzeczywistości nie chcesz, aby program `Seed` wstawiał dane testowe, jeśli będziesz używać migracji do wdrożenia bazy danych w środowisku produkcyjnym, ponieważ metoda `Seed` zostanie uruchomiona w środowisku produkcyjnym. W takim przypadku należy użyć metody `Seed`, aby wstawić do bazy danych tylko dane, które mają zostać wstawione w środowisku produkcyjnym. Na przykład baza danych może zawierać rzeczywiste nazwy działów w tabeli `Department`, gdy aplikacja będzie dostępna w środowisku produkcyjnym.

Na potrzeby tego samouczka będziesz używać migracji do wdrożenia, ale metoda `Seed` będzie wstawiać dane testowe mimo to, aby ułatwić sprawdzenie, jak działają funkcje aplikacji bez konieczności ręcznego wstawiania dużej ilości danych.

1. Zastąp zawartość pliku *Configuration.cs* następującym kodem, który spowoduje załadowanie danych testowych do nowej bazy danych. 

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

    Metoda [inicjatora](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) przyjmuje obiekt kontekstu bazy danych jako parametr wejściowy, a kod w metodzie używa tego obiektu do dodawania nowych jednostek do bazy danych. Dla każdego typu jednostki, kod tworzy kolekcję nowych jednostek, dodaje je do odpowiedniej właściwości [nieogólnymi](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) , a następnie zapisuje zmiany w bazie danych. Nie jest konieczne wywoływanie metody [metody SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) po każdej grupie jednostek, podobnie jak w tym miejscu, ale ułatwia to znalezienie źródła problemu w przypadku wystąpienia wyjątku podczas zapisu w bazie danych.

    Niektóre instrukcje, które wstawiają dane, używają metody [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) w celu wykonania operacji "upsert". Ponieważ metoda `Seed` jest uruchamiana przy każdej migracji, nie można tylko wstawić danych, ponieważ wiersze, które próbujesz dodać, będą już znajdować się po pierwszej migracji, która tworzy bazę danych. Operacja "upsert" zapobiega błędom, które mogą wystąpić w przypadku próby wstawienia wiersza, który już istnieje, ale ***zastępuje*** wszelkie zmiany danych, które mogły zostać wprowadzone podczas testowania aplikacji. Dane testowe w niektórych tabelach mogą nie być potrzebne: w niektórych przypadkach, gdy zmieniasz dane podczas testowania, jeśli chcesz, aby zmiany były nadal wykonywane po aktualizacji bazy danych. W takim przypadku należy wykonać operację warunkowego wstawiania: Wstaw wiersz tylko wtedy, gdy jeszcze nie istnieje. Metoda inicjatora używa obu metod.

    Pierwszy parametr przesłany do metody [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) określa właściwość, która ma zostać użyta do sprawdzenia, czy wiersz już istnieje. W przypadku danych studenta testowego, które udostępniasz, właściwość `LastName` może zostać użyta do tego celu, ponieważ każda Ostatnia nazwa na liście jest unikatowa:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

    Ten kod zakłada, że ostatnie nazwy są unikatowe. Po ręcznym dodaniu ucznia z duplikatem nazwiska podczas następnego przeprowadzenia migracji zostanie wyświetlony następujący wyjątek.

    Sekwencja zawiera więcej niż jeden element

    Aby uzyskać więcej informacji o metodzie `AddOrUpdate`, zobacz temat zapoznaj [się z artykułem dr 4,3 AddOrUpdate with](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) The Julie Lerman.

    Kod, który dodaje `Enrollment` jednostek, nie używa metody `AddOrUpdate`. Sprawdza, czy jednostka już istnieje i wstawia jednostkę, jeśli nie istnieje. Takie podejście spowoduje zachowanie zmian wprowadzonych w klasie rejestracji podczas uruchamiania migracji. Kod jest przełączany przez każdy element członkowski [listy](https://msdn.microsoft.com/library/6sh2ey19.aspx) `Enrollment`i jeśli rejestracja nie zostanie znaleziona w bazie danych, zostanie dodana Rejestracja do bazy danych programu. Przy pierwszym aktualizowaniu bazy danych baza danych będzie pusta, co spowoduje dodanie każdej rejestracji.

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

    Aby uzyskać informacje na temat sposobu debugowania metody `Seed` i sposobu obsługi nadmiarowych danych, takich jak dwie uczniów o nazwie "Alexander Carson", zobacz temat Umieszczanie [i debugowanie Entity Framework (EF) baz danych](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) na blogu Rick Anderson.
2. Skompiluj projekt.

### <a name="create-and-execute-the-first-migration"></a>Tworzenie i wykonywanie pierwszej migracji

1. W oknie Konsola Menedżera pakietów wprowadź następujące polecenia: 

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample14.ps1)]

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

    Polecenie `add-migration` dodaje do folderu migrations *[DateStamp]\_plik InitialCreate.cs* zawierający kod, który tworzy bazę danych. Pierwszy parametr (`InitialCreate)` jest używany jako nazwa pliku i może być dowolnie odpowiedni. zazwyczaj wybierasz słowo lub frazę, która podsumowuje zawartość wykonywaną podczas migracji. Można na przykład nazwać późniejszej migracji &quot;adddepartment&quot;.

    ![Folder migracji z początkową migracją](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

    Metoda `Up` klasy `InitialCreate` tworzy tabele bazy danych, które odpowiadają zestawom jednostek modelu danych, a metoda `Down` usuwa je. Migracja wywołuje metodę `Up`, aby zaimplementować zmiany modelu danych dla migracji. Po wprowadzeniu polecenia w celu wycofania aktualizacji migracja wywołuje metodę `Down`. Poniższy kod przedstawia zawartość pliku `InitialCreate`:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

    `update-database` polecenie uruchamia metodę `Up`, aby utworzyć bazę danych, a następnie uruchamia metodę `Seed`, aby wypełnić bazę danych.

Baza danych SQL Server została teraz utworzona dla modelu danych. Nazwa bazy danych to *ContosoUniversity*, a plik *. mdf* znajduje się w folderze *danych\_aplikacji* projektu, ponieważ jest to określone w parametrach połączenia.

Aby wyświetlić bazę danych w programie Visual Studio, można użyć opcji **Eksplorator serwera** lub **Eksplorator obiektów SQL Server** (SSOX). Na potrzeby tego samouczka będziesz używać **Eksplorator serwera**. W Visual Studio Express 2012 dla sieci Web **Eksplorator serwera** jest nazywana **Eksplorator bazy danych**.

1. W menu **Widok** kliknij **Eksplorator serwera**.
2. Kliknij ikonę **Dodaj połączenie** .

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)
3. Jeśli zostanie wyświetlony monit z oknem dialogowym **Wybieranie źródła danych** , kliknij przycisk **Microsoft SQL Server**, a następnie kliknij przycisk **Kontynuuj**.  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
4. W oknie dialogowym **Dodawanie połączenia** w polu **Nazwa serwera**wprowadź **(LocalDB) \v11.0** . W obszarze **Wybierz lub wprowadź nazwę bazy danych**wybierz pozycję **ContosoUniversity.**  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
5. Kliknij przycisk **OK**.
6. Rozwiń węzeł **SchoolContext** , a następnie rozwiń węzeł **tabele**.  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image18.png)
7. Kliknij prawym przyciskiem myszy tabelę **uczniów** i kliknij polecenie **Pokaż dane tabeli** , aby wyświetlić utworzone kolumny i wiersze, które zostały wstawione do tabeli.

    ![Tabela uczniów](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image19.png)

## <a name="creating-a-student-controller-and-views"></a>Tworzenie kontrolera i widoków ucznia

Następnym krokiem jest utworzenie kontrolera i widoków ASP.NET MVC w aplikacji, które mogą współdziałać z jedną z tych tabel.

1. Aby utworzyć kontroler `Student`, kliknij prawym przyciskiem myszy folder **controllers** w obszarze **Eksplorator rozwiązań**, wybierz pozycję **Dodaj**, a następnie kliknij pozycję **kontroler**. W oknie dialogowym **Dodaj kontroler** wybierz poniższe opcje, a następnie kliknij przycisk **Dodaj**: 

   - Nazwa kontrolera: **StudentController**.
   - Szablon: **kontroler MVC z akcjami odczytu/zapisu i widokami korzystającymi z Entity Framework**.
   - Model klasy: **student (ContosoUniversity. models)** . (Jeśli nie widzisz tej opcji na liście rozwijanej, Skompiluj projekt i spróbuj ponownie).
   - Klasa kontekstu danych: **SchoolContext (ContosoUniversity. models)** .
   - Widoki: **Razor (cshtml)** . (Wartość domyślna).

     ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image20.png)
2. Program Visual Studio otwiera plik *Controllers\StudentController.cs* . Zostanie wyświetlona zmienna klasy, która tworzy wystąpienie obiektu kontekstu bazy danych:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

     Metoda akcji `Index` pobiera listę studentów z zestawu jednostek *studentów* , odczytując Właściwość `Students` wystąpienia kontekstu bazy danych:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]

     Widok *Student\Index.cshtml* wyświetla tę listę w tabeli:

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample18.cshtml)]
3. Naciśnij klawisze CTRL + F5, aby uruchomić projekt.

     Kliknij kartę **Students (studenci** ), aby wyświetlić dane testowe, które zostały wstawione przy użyciu metody `Seed`.

     ![Strona indeksu ucznia](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image21.png)

## <a name="conventions"></a>Konwencje

Ilość kodu, który miał zostać zapisany w celu Entity Framework być w stanie utworzyć kompletną bazę danych, jest minimalny ze względu na stosowanie *Konwencji*lub zaEntity Framework łożeń. Niektóre z nich zostały już odnotowane:

- Nazwy tabel są używane w liczbie mnogiej nazw klas jednostek.
- Nazwy właściwości jednostki są używane w nazwach kolumn.
- Właściwości jednostki o nazwach `ID` lub *classname* `ID` są rozpoznawane jako właściwości klucza podstawowego.

Zobaczysz, że konwencje mogą zostać zastąpione (na przykład określono, że nazwy tabel nie powinny być w liczbie mnogiej) i dowiesz się więcej o konwencjach i sposobach ich przesłonięcia w samouczku [Tworzenie bardziej złożonego modelu danych](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md) w dalszej części tej serii. Aby uzyskać więcej informacji, zobacz [konwencje Code First](https://msdn.microsoft.com/data/jj679962).

## <a name="summary"></a>Podsumowanie

Utworzono prostą aplikację, która używa Entity Framework i SQL Server Express do przechowywania i wyświetlania danych. W poniższym samouczku dowiesz się, jak wykonywać operacje podstawowe CRUD (tworzenie, odczytywanie, aktualizowanie i usuwanie). Możesz opuścić opinię w dolnej części tej strony. Poinformuj nas o tym, jak lubisz tę część samouczka i jak możemy to poprawić.

Linki do innych zasobów Entity Framework można znaleźć w [mapie zawartości dostęp do danych ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Next](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)

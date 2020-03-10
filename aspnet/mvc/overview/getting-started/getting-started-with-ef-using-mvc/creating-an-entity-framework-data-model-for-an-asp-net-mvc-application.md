---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: 'Samouczek: Rozpoczynanie pracy z Entity Framework 6 Code First przy użyciu MVC 5 | Microsoft Docs'
description: W tej serii samouczków dowiesz się, jak utworzyć aplikację ASP.NET MVC 5, która używa Entity Framework 6 do uzyskiwania dostępu do danych.
author: tdykstra
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: 00bc8b51-32ed-4fd3-9745-be4c2a9c1eaf
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: a00a5a3aa295c2584b90abbf931c258c9e1b58ca
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616131"
---
# <a name="tutorial-get-started-with-entity-framework-6-code-first-using-mvc-5"></a>Samouczek: Rozpoczynanie pracy z Entity Framework 6 Code First przy użyciu MVC 5

> [!NOTE]
> W przypadku nowych rozwiązań zaleca się [ASP.NET Core Razor Pages](/aspnet/core/razor-pages) za pośrednictwem kontrolerów i widoków ASP.NET MVC. Aby zapoznać się z serią samouczków podobną do tej Razor Pages, zobacz [Samouczek: wprowadzenie do Razor Pages w ASP.NET Core](/aspnet/core/tutorials/razor-pages/razor-pages-start). Nowy samouczek:
> * Jest łatwiejsze do wykonania.
> * Zapewnia więcej EF Core najlepszych rozwiązań.
> * Używa bardziej wydajnych zapytań.
> * Jest bardziej aktualny przy użyciu najnowszego interfejsu API.
> * Obejmuje więcej funkcji.
> * Jest preferowanym podejściem do tworzenia nowych aplikacji.

W tej serii samouczków dowiesz się, jak utworzyć aplikację ASP.NET MVC 5, która używa Entity Framework 6 do uzyskiwania dostępu do danych. W tym samouczku jest wykorzystywany przepływ pracy Code First. Aby uzyskać informacje na temat wybierania między Code First, Database First i Model First, zobacz [Tworzenie modelu](/ef/ef6/modeling/).

W tej serii samouczków wyjaśniono, jak skompilować przykładową aplikację firmy Contoso University. Przykładowa aplikacja to prosta witryna sieci Web University. Dzięki niej można wyświetlać i aktualizować informacje dotyczące uczniów, kursów i instruktorów. Poniżej przedstawiono dwa utworzone ekrany:

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![Edytuj uczniów](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

W tym samouczku zostaną wykonane następujące czynności:

> [!div class="checklist"]
> * Tworzenie aplikacji sieci Web MVC
> * Ustawianie stylów lokacji
> * Zainstaluj Entity Framework 6
> * Tworzenie modelu danych
> * Tworzenie kontekstu bazy danych
> * Zainicjuj bazę danych z danymi testowymi
> * Konfigurowanie programu EF 6 do korzystania z LocalDB
> * Tworzenie kontrolera i widoków
> * Wyświetlanie bazy danych

## <a name="prerequisites"></a>Wymagania wstępne

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)

## <a name="create-an-mvc-web-app"></a>Tworzenie aplikacji sieci Web MVC

1. Otwórz program Visual Studio i Utwórz C# projekt sieci Web przy użyciu szablonu **aplikacji sieci web ASP.NET (.NET Framework)** . Nazwij projekt *ContosoUniversity* i wybierz **przycisk OK**.

   ![Okno dialogowe Nowy projekt w programie Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/new-project-dialog.png)

1. W obszarze **Nowa aplikacja sieci Web ASP.NET — ContosoUniversity**wybierz pozycję **MVC**.

   ![Okno dialogowe Nowa aplikacja sieci Web w programie Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/new-web-app-dialog.png)

    > [!NOTE]
    > Domyślnie opcja **uwierzytelniania** jest ustawiona na wartość **bez uwierzytelniania**. W tym samouczku aplikacja internetowa nie wymaga od użytkowników zalogowania się. Ponadto nie ogranicza dostępu w zależności od tego, kto jest zalogowany.

1. Wybierz przycisk **OK**, aby utworzyć projekt.

## <a name="set-up-the-site-style"></a>Ustawianie stylów lokacji

Kilka prostych zmian spowoduje skonfigurowanie menu witryny, układu i strony głównej.

1. Otwórz *Views\Shared\\_Layout. cshtml*i wprowadź następujące zmiany:

   - Zmień każde wystąpienie elementu "My ASP.NET Application" i "Nazwa aplikacji" na "Uniwersytet firmy Contoso".
   - Dodaj pozycje menu dla studentów, kursów, instruktorów i działów, a następnie usuń wpis kontaktu.

   Zmiany są wyróżnione w poniższym fragmencie kodu:

   [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=6,19,24-27,38)]

2. W *Views\Home\Index.cshtml*Zastąp zawartość pliku następującym kodem, aby zastąpić tekst o ASP.NET i MVC tekstem dotyczącym tej aplikacji:

   [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

3. Naciśnij klawisze CTRL + F5, aby uruchomić witrynę sieci Web. Zostanie wyświetlona strona główna z menu głównym.

## <a name="install-entity-framework-6"></a>Zainstaluj Entity Framework 6

1. W menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet**, a następnie wybierz pozycję **konsola Menedżera pakietów**.

2. W oknie **konsola Menedżera pakietów** wprowadź następujące polecenie:

   ```text
   Install-Package EntityFramework
   ```

Ten krok to jeden z kilku kroków wykonywanych ręcznie przez ten samouczek, ale które mogły zostać wykonane automatycznie przez funkcję tworzenia szkieletu MVC ASP.NET. Wykonujesz je ręcznie, aby zobaczyć kroki wymagane do korzystania z Entity Framework (EF). Użyjesz szkieletu później, aby utworzyć kontroler MVC i widoki. Alternatywą jest umożliwienie automatycznej instalacji pakietu NuGet EF, utworzenie klasy kontekstu bazy danych i utworzenie parametrów połączenia. Gdy wszystko jest gotowe do wykonania, wystarczy pominąć te kroki i utworzyć szkielet kontrolera MVC po utworzeniu klas jednostek.

## <a name="create-the-data-model"></a>Tworzenie modelu danych

Następnie utworzysz klasy jednostek dla aplikacji firmy Contoso University. Zaczniesz od następujących trzech jednostek:

**Rejestracja** <-> **kursu** <-> **student**

| Jednostki | Relacja |
| -------- | ------------ |
| Kurs do rejestracji | Jeden do wielu |
| Student do rejestracji | Jeden do wielu |

Istnieje relacja jeden do wielu między jednostkami `Student` i `Enrollment` i istnieje relacja jeden do wielu między `Course` i `Enrollment` jednostek. Innymi słowy, student może być zarejestrowany w dowolnej liczbie kursów, a kurs może mieć dowolną liczbę uczniów zarejestrowanych w nim.

W poniższych sekcjach utworzysz klasę dla każdej z tych jednostek.

> [!NOTE]
> Jeśli spróbujesz skompilować projekt przed ukończeniem tworzenia wszystkich tych klas jednostek, uzyskasz błędy kompilatora.

### <a name="the-student-entity"></a>Jednostki dla uczniów

- W folderze *modele* Utwórz plik klasy o nazwie *student.cs* , klikając prawym przyciskiem myszy folder w **Eksplorator rozwiązań** i wybierając polecenie **Dodaj** **klasę** > . Zastąp kod szablonu poniższym kodem:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs)]

Właściwość `ID` stanie się kolumną klucza podstawowego tabeli bazy danych, która odnosi się do tej klasy. Domyślnie Entity Framework interpretuje właściwość o nazwie `ID` lub *classname* `ID` jako klucz podstawowy.

Właściwość `Enrollments` jest *właściwością nawigacji*. Właściwości nawigacji zawierają inne jednostki, które są powiązane z tą jednostką. W takim przypadku Właściwość `Enrollments` jednostki `Student` będzie zawierać wszystkie jednostki `Enrollment`, które są powiązane z tą jednostką `Student`. Innymi słowy, jeśli dany wiersz `Student` w bazie danych ma dwa powiązane `Enrollment` wiersze (wiersze, które zawierają wartość klucza podstawowego tego ucznia w `StudentID` kolumnie klucza obcego), właściwość nawigacji `Enrollments` jednostki `Student` będzie zawierać te dwie jednostki `Enrollment`.

Właściwości nawigacji są zwykle zdefiniowane jako `virtual`, dzięki czemu mogą korzystać z niektórych funkcji Entity Framework, takich jak *ładowanie z opóźnieniem*. (Ładowanie z opóźnieniem zostanie omówione później, w samouczku [odczytywanie danych pokrewnych](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) w dalszej części tej serii).

Jeśli właściwość nawigacji może zawierać wiele jednostek (tak jak w przypadku relacji "wiele do wielu" lub "jeden do wielu"), jej typem musi być lista, w której można dodawać, usuwać i aktualizować wpisy, takie jak `ICollection`.

### <a name="the-enrollment-entity"></a>Jednostki rejestracji

- W folderze *modele* Utwórz *Enrollment.cs* i Zastąp istniejący kod następującym kodem:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

Właściwość `EnrollmentID` będzie kluczem podstawowym; Ta jednostka używa wzorca *classname* `ID` zamiast `ID`, jak pokazano w jednostce `Student`. Zwykle należy wybrać jeden wzorzec i używać go w całym modelu danych. Tutaj, odmiana ilustruje, że można użyć dowolnego wzorca. W kolejnym samouczku zobaczysz, jak używać `ID` bez `classname` ułatwia implementowanie dziedziczenia w modelu danych.

Właściwość `Grade` jest [wyliczeniem](/ef/ef6/modeling/code-first/data-types/enums). Znak zapytania po deklaracji typu `Grade` wskazuje, że właściwość `Grade` ma [wartość null](/dotnet/csharp/programming-guide/nullable-types/using-nullable-types). Klasa o wartości null różni się od klasy zerowej — wartość null oznacza, że Klasa nie jest znana lub nie została jeszcze przypisana.

Właściwość `StudentID` jest kluczem obcym, a odpowiednia właściwość nawigacji jest `Student`. Jednostka `Enrollment` jest skojarzona z jedną jednostką `Student`, więc właściwość może zawierać tylko jedną jednostkę `Student` (w przeciwieństwie do `Student.Enrollments`j właściwości nawigacji, która może być przechowywana w wielu jednostkach `Enrollment`).

Właściwość `CourseID` jest kluczem obcym, a odpowiednia właściwość nawigacji jest `Course`. Jednostka `Enrollment` jest skojarzona z jedną jednostką `Course`.

Entity Framework interpretuje właściwość jako właściwość klucza obcego, jeśli jest nazywana *&lt;nazwą właściwości nawigacji&gt;&lt;nazwą właściwości klucza podstawowego&gt;* (na przykład `StudentID` właściwości nawigacji `Student`, ponieważ klucz podstawowy jednostki `Student` to `ID`). Właściwości klucza obcego mogą również mieć taką samą *nazwę&lt;nazwy właściwości klucza podstawowego&gt;* (na przykład `CourseID`, ponieważ klucz podstawowy jednostki `Course` to `CourseID`).

### <a name="the-course-entity"></a>Jednostki kursu

- W folderze *modele* Utwórz *Course.cs*, zastępując kod szablonu następującym kodem:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

Właściwość `Enrollments` jest właściwością nawigacji. Jednostka `Course` może być powiązana z dowolną liczbą `Enrollment` jednostek.

Dowiesz się więcej o atrybucie <xref:System.ComponentModel.DataAnnotations.Schema.DatabaseGeneratedAttribute> w późniejszym samouczku w tej serii. Zasadniczo ten atrybut umożliwia wprowadzenie klucza podstawowego dla kursu, a nie jego wygenerowanie.

## <a name="create-the-database-context"></a>Tworzenie kontekstu bazy danych

Klasa główna, która koordynuje funkcje Entity Framework dla danego modelu danych, jest klasą *kontekstu bazy danych* . Tę klasę można utworzyć, pobierając ją z klasy [System. Data. Entity. DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.113).aspx) . W kodzie możesz określić, które jednostki zostaną uwzględnione w modelu danych. Można również dostosować pewne zachowanie Entity Framework. W tym projekcie Klasa ma nazwę `SchoolContext`.

- Aby utworzyć folder w projekcie ContosoUniversity, kliknij prawym przyciskiem myszy projekt w **Eksplorator rozwiązań** a następnie kliknij pozycję **Dodaj**, a następnie kliknij pozycję **Nowy folder**. Nazwij nowy folder *dal* (na potrzeby warstwy dostępu do danych). W tym folderze Utwórz nowy plik klasy o nazwie *SchoolContext.cs*i Zastąp kod szablonu następującym kodem:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

### <a name="specify-entity-sets"></a>Określ zestawy jednostek

Ten kod tworzy właściwość [nieogólnymi](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.113).aspx) dla każdego zestawu jednostek. W Entity Framework terminologii *zestaw jednostek* zwykle odpowiada tabeli bazy danych, a *Jednostka* odpowiada wierszowi w tabeli.

> [!NOTE]
>
> Możesz pominąć instrukcje `DbSet<Enrollment>` i `DbSet<Course>`, aby działały tak samo. Entity Framework będzie zawierać je niejawnie, ponieważ jednostka `Student` odwołuje się do jednostki `Enrollment`, a jednostka `Enrollment` odwołuje się do jednostki `Course`.

### <a name="specify-the-connection-string"></a>Określ parametry połączenia

Nazwa parametrów połączenia (które zostaną dodane do pliku Web. config później) jest przenoszona do konstruktora.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=1)]

Można również przekazać parametry połączenia zamiast nazwy, która jest przechowywana w pliku Web. config. Aby uzyskać więcej informacji na temat opcji określania bazy danych do użycia, zobacz [Parametry połączenia i modele](/ef/ef6/fundamentals/configuring/connection-strings).

Jeśli nie określisz jawnie parametrów połączenia lub nazwy, Entity Framework zakłada, że nazwa parametrów połączenia jest taka sama jak nazwa klasy. Domyślną nazwą parametrów połączenia w tym przykładzie będzie `SchoolContext`, tak samo jak to, co jest określane jawnie.

### <a name="specify-singular-table-names"></a>Określanie pojedynczej nazwy tabeli

Instrukcja `modelBuilder.Conventions.Remove` w metodzie [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) zapobiega dokonywaniu pluralizmu nazw tabel. Jeśli nie zostało to zrobione, wygenerowane tabele w bazie danych byłyby nazwane `Students`, `Courses`i `Enrollments`. Zamiast tego nazwy tabel będą `Student`, `Course`i `Enrollment`. Deweloperzy nie zgadzają się, czy nazwy tabel powinny być w liczbie mnogiej. W tym samouczku jest używana forma Jednoargumentowa, ale ważnym punktem jest to, że można wybrać dowolną formę, wybierając lub pomijając ten wiersz kodu.

## <a name="initialize-db-with-test-data"></a>Zainicjuj bazę danych z danymi testowymi

Entity Framework może automatycznie utworzyć lub porzucić i ponownie utworzyć bazę danych, gdy aplikacja zostanie uruchomiona. Można określić, że należy to zrobić za każdym razem, gdy aplikacja jest uruchamiana, lub tylko wtedy, gdy model nie jest zsynchronizowany z istniejącą bazą danych. Możesz również napisać `Seed` metodę, która Entity Framework automatycznie wywołuje po utworzeniu bazy danych w celu wypełnienia jej danymi testowymi.

Domyślnym zachowaniem jest utworzenie bazy danych tylko wtedy, gdy nie istnieje (i zgłosić wyjątek, jeśli model został zmieniony, a baza danych już istnieje). W tej sekcji określisz, że baza danych powinna zostać porzucona i utworzona w momencie zmiany modelu. Porzucenie bazy danych spowoduje utratę wszystkich danych. Jest to ogólnie dobry podczas opracowywania, ponieważ metoda `Seed` zostanie uruchomiona, gdy baza danych zostanie ponownie utworzona i ponownie utworzy dane testowe. Jednak w środowisku produkcyjnym zwykle nie chcesz utracić wszystkich danych za każdym razem, gdy trzeba zmienić schemat bazy danych. Później zobaczysz, jak obsługiwać zmiany modelu przy użyciu Migracje Code First, aby zmienić schemat bazy danych zamiast upuszczania i ponownego tworzenia bazy danych.

1. W folderze DAL Utwórz nowy plik klasy o nazwie *SchoolInitializer.cs* i Zastąp kod szablonu następującym kodem, co spowoduje utworzenie bazy danych w razie potrzeby i załadowanie danych testowych do nowej bazy danych.

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

   Metoda `Seed` przyjmuje obiekt kontekstu bazy danych jako parametr wejściowy, a kod w metodzie używa tego obiektu do dodawania nowych jednostek do bazy danych. Dla każdego typu jednostki, kod tworzy kolekcję nowych jednostek, dodaje do odpowiedniej właściwości `DbSet`, a następnie zapisuje zmiany w bazie danych. Nie jest konieczne wywoływanie metody `SaveChanges` po każdej grupie jednostek, jak zostało to zrobione, ale to ułatwia znalezienie źródła problemu w przypadku wystąpienia wyjątku podczas zapisywania w bazie danych.

2. Aby poinformować Entity Framework o użyciu klasy inicjatora, Dodaj element do `entityFramework` elementu w pliku *Web. config* aplikacji (znajdującym się w folderze głównym projektu), jak pokazano w następującym przykładzie:

   [!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.xml?highlight=2-6)]

   `context type` określa w pełni kwalifikowaną nazwę klasy kontekstu i zestaw, w której znajduje się, a `databaseinitializer type` określa w pełni kwalifikowaną nazwę klasy inicjatora i zestawu, w którym się znajduje. (Jeśli nie chcesz używać inicjatora EF, możesz ustawić atrybut dla elementu `context`: `disableDatabaseInitialization="true"`). Aby uzyskać więcej informacji, zobacz [Ustawienia pliku konfiguracji](/ef/ef6/fundamentals/configuring/config-file).

   Alternatywą dla ustawienia inicjatora w pliku *Web. config* jest to zrobić w kodzie poprzez dodanie instrukcji `Database.SetInitializer` do metody `Application_Start` w pliku *Global.asax.cs* . Aby uzyskać więcej informacji, zobacz [Omówienie inicjatorów bazy danych w Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm).

Aplikacja została skonfigurowana tak, aby po raz pierwszy uzyskać dostęp do bazy danych w danym przebiegu aplikacji, Entity Framework porównuje bazę danych z modelem (`SchoolContext` i klasy jednostek). Jeśli istnieje różnica, aplikacja pospadnie i utworzy ponowną bazę danych.

> [!NOTE]
> Podczas wdrażania aplikacji na produkcyjnym serwerze sieci Web należy usunąć lub wyłączyć kod, który opuszcza i ponownie tworzy bazę danych. Należy to zrobić w kolejnym samouczku w tej serii.

## <a name="set-up-ef-6-to-use-localdb"></a>Konfigurowanie programu EF 6 do korzystania z LocalDB

[LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb?view=sql-server-2017) to uproszczona wersja aparatu bazy danych SQL Server Express. Można łatwo instalować i konfigurować, uruchamiać na żądanie i uruchamiać w trybie użytkownika. LocalDB działa w specjalnym trybie wykonywania SQL Server Express, który umożliwia współpracę z bazami danych jako plikami *MDF* . Pliki bazy danych LocalDB można umieścić w folderze *danych\_aplikacji* projektu sieci Web, jeśli chcesz mieć możliwość skopiowania bazy danych do projektu. Funkcja wystąpienia użytkownika w SQL Server Express umożliwia również pracy z plikami *. mdf* , ale funkcja wystąpienia użytkownika jest przestarzała; w związku z tym LocalDB jest zalecane do pracy z plikami *. mdf* . LocalDB jest instalowany domyślnie z programem Visual Studio.

Zwykle SQL Server Express nie jest używany dla produkcyjnych aplikacji sieci Web. LocalDB w szczególności nie są zalecane do użycia w środowisku produkcyjnym z aplikacją sieci Web, ponieważ nie jest ona przeznaczona do pracy z usługami IIS.

- W tym samouczku będziesz korzystać z LocalDB. Otwórz plik *Web. config* aplikacji i Dodaj `connectionStrings` element poprzedzający element `appSettings`, jak pokazano w poniższym przykładzie. (Pamiętaj, aby zaktualizować plik *Web. config* w folderze głównym projektu. Istnieje również plik *Web. config* w podfolderze *widoki* , które nie są potrzebne do aktualizacji.

   [!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.xml?highlight=1-3)]

Dodawane parametry połączenia określają, że Entity Framework będzie używać bazy danych LocalDB o nazwie *ContosoUniversity1. mdf*. (Baza danych nie istnieje jeszcze, ale program Dr utworzy ją.) Jeśli chcesz utworzyć bazę danych w folderze *danych\_aplikacji* , możesz dodać `AttachDBFilename=|DataDirectory|\ContosoUniversity1.mdf` do parametrów połączenia. Aby uzyskać więcej informacji dotyczących parametrów połączenia, zobacz [SQL Server parametry połączenia dla aplikacji sieci Web ASP.NET](/previous-versions/aspnet/jj653752(v=vs.110)).

W pliku *Web. config* nie są potrzebne parametry połączenia. Jeśli nie podasz parametrów połączenia, Entity Framework używa domyślnych parametrów połączenia opartych na klasie kontekstowej. Aby uzyskać więcej informacji, zobacz [Code First do nowej bazy danych](/ef/ef6/modeling/code-first/workflows/new-database).

## <a name="create-controller-and-views"></a>Tworzenie kontrolera i widoków

Teraz utworzysz stronę sieci Web, aby wyświetlić dane. Proces żądania danych automatycznie wyzwala Tworzenie bazy danych. Zacznij od utworzenia nowego kontrolera. Jednak przed wykonaniem tej czynności Skompiluj projekt, aby udostępnić klasy modelu i kontekstu dla szkieletu kontrolera MVC.

1. Kliknij prawym przyciskiem myszy folder **controllers** w **Eksplorator rozwiązań**, wybierz pozycję **Dodaj**, a następnie kliknij pozycję **nowy element szkieletowy**.
2. W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **kontroler MVC 5 z widokami, używając Entity Framework**, a następnie wybierz pozycję **Dodaj**.

     ![Okno dialogowe Dodawanie szkieletu w programie Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/add-scaffold.png)

3. W oknie dialogowym **Dodaj kontroler** wprowadź następujące opcje, a następnie wybierz pozycję **Dodaj**:

   - Model klasy: **student (ContosoUniversity. models)** . (Jeśli nie widzisz tej opcji na liście rozwijanej, Skompiluj projekt i spróbuj ponownie).
   - Klasa kontekstu danych: **SchoolContext (ContosoUniversity. dal)** .
   - Nazwa kontrolera: **StudentController** (nie StudentsController).
   - Pozostaw wartości domyślne dla pozostałych pól.

     Po kliknięciu przycisku **Dodaj**tworzy plik *StudentController.cs* i zestaw widoków (pliki *. cshtml* ), które współpracują z kontrolerem. W przyszłości podczas tworzenia projektów, które używają Entity Framework, można także skorzystać z dodatkowych funkcji szkieletu: Utwórz pierwszą klasę modelu, nie twórz parametrów połączenia, a następnie w polu **Dodaj kontroler** Określ **nowy kontekst danych** , wybierając przycisk **+** obok **klasy kontekstu danych**. Program tworzący szkielet utworzy klasę `DbContext` i parametry połączenia, jak również kontroler i widoki.
4. Program Visual Studio otwiera plik *Controllers\StudentController.cs* . Zobaczysz, że została utworzona zmienna klasy, która tworzy wystąpienie obiektu kontekstu bazy danych:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

     Metoda akcji `Index` pobiera listę studentów z zestawu jednostek *studentów* , odczytując Właściwość `Students` wystąpienia kontekstu bazy danych:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

     Widok *Student\Index.cshtml* wyświetla tę listę w tabeli:

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cshtml)]
5. Naciśnij klawisze CTRL + F5, aby uruchomić projekt. (Jeśli zostanie wyświetlony komunikat o błędzie "nie można utworzyć kopii w tle", zamknij przeglądarkę i spróbuj ponownie).

     Kliknij kartę **Students (studenci** ), aby wyświetlić dane testowe, które zostały wstawione przy użyciu metody `Seed`. W zależności od tego, jak Zawężasz okno przeglądarki, zobaczysz link do karty uczniów na górnym pasku adresu lub kliknij prawym górnym rogu, aby zobaczyć link.

     ![Przycisk menu](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

## <a name="view-the-database"></a>Wyświetlanie bazy danych

Po uruchomieniu strony uczniów, gdy aplikacja próbowała uzyskać dostęp do bazy danych, EF wykryła, że nie istniała baza danych i została utworzona. EF następnie uruchomił metodę inicjatora, aby wypełnić bazę danych danymi.

Aby wyświetlić bazę danych w programie Visual Studio, można użyć opcji **Eksplorator serwera** lub **Eksplorator obiektów SQL Server** (SSOX). Na potrzeby tego samouczka będziesz używać **Eksplorator serwera**.

1. Zamknij okno przeglądarki.
2. W **Eksplorator serwera**rozwiń węzeł **połączenia danych** (może być konieczne wybranie najpierw przycisku Odśwież), rozwinięcie **kontekstu szkoły (ContosoUniversity)** , a następnie rozwiń węzeł **tabele** , aby wyświetlić tabele w nowej bazie danych.

3. Kliknij prawym przyciskiem myszy tabelę **uczniów** i kliknij polecenie **Pokaż dane tabeli** , aby wyświetlić utworzone kolumny i wiersze, które zostały wstawione do tabeli.

4. Zamknij **Eksplorator serwera** połączenie.

Pliki bazy danych *ContosoUniversity1. mdf* i *. ldf* znajdują się w folderze *% USERPROFILE%* .

Ponieważ używasz inicjatora `DropCreateDatabaseIfModelChanges`, możesz teraz wprowadzić zmianę klasy `Student`, ponownie uruchomić aplikację, a baza danych zostanie automatycznie utworzona w celu dopasowania do zmiany. Na przykład jeśli dodasz Właściwość `EmailAddress` do klasy `Student`, ponownie uruchom stronę uczniów, a następnie ponownie Przyjrzyjmy się tabeli, zobaczysz nową kolumnę `EmailAddress`.

## <a name="conventions"></a>Konwencja

Ilość kodu, który miał zostać zapisany w celu Entity Framework być w stanie utworzyć kompletną bazę danych, jest minimalny ze względu na *konwencje*lub założenia, które Entity Framework. Niektóre z nich zostały już notowane lub zostały użyte bez informacji o nich:

- Nazwy tabel są używane w liczbie mnogiej nazw klas jednostek.
- Nazwy właściwości jednostki są używane w nazwach kolumn.
- Właściwości jednostki o nazwach `ID` lub *classname* `ID` są rozpoznawane jako właściwości klucza podstawowego.
- Właściwość jest interpretowana jako właściwość klucza obcego, jeśli jest nazywana *&lt;nazwy właściwości nawigacji&gt;&lt;nazwy właściwości klucza podstawowego&gt;* (na przykład `StudentID` właściwości nawigacji `Student`, ponieważ klucz podstawowy jednostki `Student` to `ID`). Właściwości klucza obcego mogą również mieć taką samą nazwę &lt;nazwy właściwości klucza podstawowego&gt; (na przykład `EnrollmentID`, ponieważ klucz podstawowy jednostki `Enrollment` to `EnrollmentID`).

Zobaczysz, że konwencje mogą zostać zastąpione. Na przykład należy określić, że nazwy tabel nie powinny być przerzucane, a następnie zobaczyć, jak jawnie oznaczyć właściwość jako właściwość klucza obcego.

## <a name="get-the-code"></a>Uzyskiwanie kodu

[Pobierz ukończony projekt](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Dodatkowe zasoby

Aby uzyskać więcej informacji na temat programu EF 6, zobacz następujące artykuły:

* [Dostęp do danych na platformie ASP.NET — zalecane zasoby](../../../../whitepapers/aspnet-data-access-content-map.md)

* [Konwencje Code First](/ef/ef6/modeling/code-first/conventions/built-in)

* [Tworzenie bardziej złożonego modelu danych](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)

## <a name="next-steps"></a>Następne kroki

W tym samouczku zostaną wykonane następujące czynności:

> [!div class="checklist"]
> * Utworzono aplikację sieci Web MVC
> * Ustawianie stylów lokacji
> * Zainstalowano Entity Framework 6
> * Utworzono model danych
> * Utworzono kontekst bazy danych
> * Zainicjowano bazę danych z danymi testowymi
> * Konfigurowanie programu EF 6 do korzystania z LocalDB
> * Utworzono kontroler i widoki
> * Wyświetlanie bazy danych

Przejdź do następnego artykułu, aby dowiedzieć się, jak przeglądać i dostosowywać kod Create, Read, Update, Delete (CRUD) w kontrolerach i widokach.
> [!div class="nextstepaction"]
> [Zaimplementuj podstawowe funkcje CRUD](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
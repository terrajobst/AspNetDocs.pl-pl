---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
title: 'Korzystając z Entity Framework 4,0 i formantu ObjectDataSource, część 1: Wprowadzenie | Microsoft Docs'
author: tdykstra
description: Ta seria samouczków jest oparta na aplikacji sieci Web firmy Contoso University, utworzonej przez Wprowadzenie z serią samouczków Entity Framework. Jeśli yo...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 244278c1-fec8-4255-8a8a-13bde491c4f5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
msc.type: authoredcontent
ms.openlocfilehash: 2f14707eb058d438495dd2bc4c17b976c471fc97
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78547293"
---
# <a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-1-getting-started"></a>Używanie Entity Framework 4,0 i formantu ObjectDataSource, część 1: Wprowadzenie

Autor [Dykstra](https://github.com/tdykstra)

> Ta seria samouczków jest oparta na aplikacji sieci Web firmy Contoso University, utworzonej przez Wprowadzenie z serią samouczków [Entity Framework 4,0](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) . Jeśli wcześniej samouczki nie zostały wykonane, jako punkt wyjścia dla tego samouczka możesz [pobrać utworzoną aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) . Możesz również [pobrać aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) utworzoną przez kompletną serię samouczków.
> 
> Przykładowa aplikacja internetowa Contoso University pokazuje, jak tworzyć aplikacje ASP.NET Web Forms przy użyciu Entity Framework 4,0 i programu Visual Studio 2010. Przykładowa aplikacja jest witryną internetową fikcyjnej firmy Contoso University. Obejmuje funkcje, takie jak czasowej dla uczniów, tworzenia kurs i przypisania instruktora.
> 
> W tym samouczku przedstawiono C#przykłady w temacie. [Przykład do pobrania](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) zawiera kod w obu C# i Visual Basic.
> 
> ## <a name="database-first"></a>Database First
> 
> Istnieją trzy sposoby pracy z danymi w Entity Framework: *Database First*, *model First*i *Code First*. Ten samouczek jest przeznaczony dla Database First. Aby uzyskać informacje o różnicach między tymi przepływami pracy i wskazówkami dotyczącymi sposobu wybierania najlepszego z nich dla danego scenariusza, zobacz [Entity Frameworke przepływy pracy projektowania](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>Formularze sieci Web
> 
> Podobnie jak w przypadku serii Wprowadzenie, ta seria samouczków korzysta z modelu ASP.NET Web Forms i zakłada, że wiesz, jak korzystać z formularzy sieci Web ASP.NET w programie Visual Studio. Jeśli nie, zobacz [wprowadzenie z formularzami sieci Web ASP.NET 4,5](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Jeśli wolisz pracować z platformą MVC ASP.NET, zobacz [wprowadzenie z Entity Framework przy użyciu ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="software-versions"></a>Wersje oprogramowania
> 
> | **Pokazane w samouczku** | **Współpracuje również z** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Visual Studio 2010 Express for Web. Samouczek nie został przetestowany z nowszymi wersjami programu Visual Studio. Istnieje wiele różnic w wyborze menu, oknach dialogowych i szablonach. |
> | .NET 4 | Program .NET 4,5 jest zgodny z platformą .NET 4, ale samouczek nie został przetestowany z platformą .NET 4,5. |
> | Entity Framework 4 | Samouczek nie został przetestowany z nowszymi wersjami Entity Framework. Począwszy od Entity Framework 5, EF używa domyślnie `DbContext API`, które zostały wprowadzone z EF 4,1. Formant EntityDataSource został zaprojektowany tak, aby korzystał z interfejsu API `ObjectContext`. Aby uzyskać informacje o sposobach używania kontrolki EntityDataSource z interfejsem API `DbContext`, zobacz [ten wpis w blogu](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx). |
> 
> ## <a name="questions"></a>Masz
> 
> Jeśli masz pytania, które nie są bezpośrednio związane z samouczkiem, możesz je ogłosić na [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), [forum Entity Framework i LINQ to Entities](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/)lub [StackOverflow.com](http://stackoverflow.com/).

Kontrolka `EntityDataSource` umożliwia szybkie tworzenie aplikacji, ale zazwyczaj wymaga, aby zachować znaczną logikę biznesową i logikę dostępu do danych na stronach *. aspx* . Jeśli spodziewasz się, że aplikacja zostanie powiększona o złożoność i będzie wymagała trwającej konserwacji, możesz zainwestować więcej czasu deweloperskiego w celu utworzenia warstwy *n-warstwowej* lub *warstwowej* aplikacji, która jest bardziej utrzymywana. Aby zaimplementować tę architekturę, należy oddzielić warstwę prezentacji od warstwy logiki biznesowej (LOGIKI biznesowej) i warstwy dostępu do danych (DAL). Jednym ze sposobów implementacji tej struktury jest użycie kontrolki `ObjectDataSource` zamiast kontrolki `EntityDataSource`. Gdy używasz kontrolki `ObjectDataSource`, zaimplementujsz własny kod dostępu do danych, a następnie wywołajesz go na stronach *. aspx* przy użyciu kontrolki zawierającej wiele takich samych funkcji, jak inne kontrolki źródła danych. Dzięki temu można łączyć zalety wielowarstwowego podejścia z korzyściami z używania kontrolki formularzy sieci Web do uzyskiwania dostępu do danych.

Formant `ObjectDataSource` zapewnia większą elastyczność, a także inne sposoby. Ponieważ piszesz własny kod dostępu do danych, łatwiej jest wykonywać więcej niż tylko odczyt, wstawianie, aktualizowanie lub usuwanie określonego typu jednostki, które są zadaniami, które są przeznaczone do wykonania przez formant `EntityDataSource`. Można na przykład przeprowadzić rejestrację za każdym razem, gdy jednostka jest aktualizowana, archiwizować dane przy każdym usunięciu jednostki lub automatycznie sprawdzać i aktualizować powiązane dane w miarę potrzeby podczas wstawiania wiersza z wartością klucza obcego.

## <a name="business-logic-and-repository-classes"></a>Klasy logiki biznesowej i repozytorium

Kontrolka `ObjectDataSource` działa przez wywoływanie utworzonej klasy. Klasa zawiera metody, które pobierają i aktualizują dane, i podajesz nazwy tych metod do kontrolki `ObjectDataSource` w znaczniku. Podczas renderowania lub przetwarzania ogłaszania zwrotnego, `ObjectDataSource` wywołuje metody, które zostały określone.

Poza podstawowymi operacjami CRUD, Klasa tworzona do użycia z kontrolką `ObjectDataSource` może być konieczne wykonanie logiki biznesowej, gdy `ObjectDataSource` odczytuje lub aktualizuje dane. Na przykład podczas aktualizowania działu może być konieczne zweryfikowanie, że żadne inne działy nie mają tego samego administratora, ponieważ jedna osoba nie może być administratorem więcej niż jednego działu.

W niektórych `ObjectDataSource` dokumentacji, takich jak [klasy ObjectDataSource](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.aspx), formant wywołuje klasę, do której odwołuje się *obiekt biznesowy* , który obejmuje logikę biznesową i logikę dostępu do danych. W tym samouczku utworzysz oddzielne klasy dla logiki biznesowej i dla logiki dostępu do danych. Klasa, która hermetyzuje logikę dostępu do danych, nazywa się *repozytorium*. Klasa logiki biznesowej obejmuje metody logiki biznesowej i metody dostępu do danych, ale metody dostępu do danych wywołują repozytorium w celu wykonywania zadań dostępu do danych.

Utworzysz również warstwę abstrakcji między LOGIKI biznesowej i DAL, które ułatwiają zautomatyzowane testowanie jednostkowe LOGIKI biznesowej. Ta warstwa abstrakcji jest implementowana przez utworzenie interfejsu i użycie interfejsu podczas tworzenia wystąpienia repozytorium w klasie logiki biznesowej. Dzięki temu można dostarczyć klasę logiki biznesowej z odwołaniem do dowolnego obiektu, który implementuje interfejs repozytorium. W przypadku normalnej operacji należy podać obiekt repozytorium, który współdziała z Entity Framework. W celu przetestowania należy podać obiekt repozytorium, który współdziała z danymi przechowywanymi w sposób, który można łatwo manipulować, na przykład w przypadku zmiennych klas zdefiniowanych jako kolekcje.

Na poniższej ilustracji przedstawiono różnicę między klasą logiki biznesowej, która obejmuje logikę dostępu do danych bez repozytorium i jeden, który używa repozytorium.

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image1.png)

Rozpocznie się tworzenie stron sieci Web, w których formant `ObjectDataSource` jest powiązany bezpośrednio z repozytorium, ponieważ wykonuje tylko podstawowe zadania dostępu do danych. W następnym samouczku utworzysz klasę logiki biznesowej z logiką walidacji i powiążesz formant `ObjectDataSource` z tą klasą, a nie z klasą repozytorium. Będziesz również tworzyć testy jednostkowe dla logiki walidacji. W trzecim samouczku w tej serii zostanie dodana funkcja sortowania i filtrowania do aplikacji.

Strony utworzone w tym samouczku współpracują z zestawem jednostek `Departments` modelu danych utworzonego w [serii samouczków wprowadzenie](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md).

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image3.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image5.png)

## <a name="updating-the-database-and-the-data-model"></a>Aktualizowanie bazy danych i modelu danych

Ten samouczek rozpoczyna się od wprowadzenia dwóch zmian w bazie danych, które wymagają odpowiednich zmian w modelu danych, który został utworzony w [wprowadzenie za pomocą samouczków Entity Framework i formularzy sieci Web](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) . W jednym z tych samouczków ręcznie wprowadzono zmiany w projektancie, aby zsynchronizować model danych z bazą danych po zmianie bazy danych. W tym samouczku użyjesz **modelu aktualizacji projektanta z Narzędzia bazy danych** , aby automatycznie aktualizować model danych.

### <a name="adding-a-relationship-to-the-database"></a>Dodawanie relacji do bazy danych

W programie Visual Studio Otwórz aplikację sieci Web Contoso University utworzoną w Wprowadzenie z serią samouczków [Entity Framework i Web Forms](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) , a następnie Otwórz diagram bazy danych `SchoolDiagram`.

Jeśli przyjrzyjsz się tabeli `Department` w diagramie bazy danych, zobaczysz, że ma ona `Administrator` kolumnę. Ta kolumna jest kluczem obcym tabeli `Person`, ale w bazie danych nie zdefiniowano relacji klucza obcego. Należy utworzyć relację i zaktualizować model danych, tak aby Entity Framework mógł automatycznie obsłużyć tę relację.

W diagramie bazy danych kliknij prawym przyciskiem myszy tabelę `Department` i wybierz pozycję **relacje**.

[![Image80](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image7.png)

W polu **relacje klucza obcego** kliknij przycisk **Dodaj**, a następnie kliknij przycisk wielokropka dla **specyfikacji tabele i kolumny**.

[![Image81](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image9.png)

W oknie dialogowym **tabele i kolumny** Ustaw tabelę klucza podstawowego i pole na `Person` i `PersonID`, a następnie ustaw tabelę klucza obcego i pole na `Department` i `Administrator`. (W takim przypadku nazwa relacji zmieni się z `FK_Department_Department` na `FK_Department_Person`).

[![Image82](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image11.png)

Kliknij przycisk **OK** w polu **tabele i kolumny** , kliknij przycisk **Zamknij** w polu **relacje klucza obcego** i Zapisz zmiany. Jeśli zostanie wyświetlony monit o zapisanie tabel `Person` i `Department`, kliknij przycisk **tak**.

> [!NOTE]
> Jeśli usunięto `Person` wierszy, które odpowiadają danych znajdujących się już w kolumnie `Administrator`, nie będzie można zapisać tej zmiany. W takim przypadku należy użyć edytora tabel w **Eksplorator serwera** , aby upewnić się, że wartość `Administrator` w każdym `Department` wierszu zawiera identyfikator rekordu, który faktycznie istnieje w tabeli `Person`.
> 
> Po zapisaniu zmiany nie będzie można usunąć wiersza z tabeli `Person`, jeśli ta osoba jest administratorem działu. W aplikacji produkcyjnej można podać określony komunikat o błędzie, gdy ograniczenie bazy danych uniemożliwia usunięcie lub należy określić kaskadowe usuwanie. Aby zapoznać się z przykładem sposobu określania kaskadowego usuwania, zobacz [Entity Framework i ASP.NET – wprowadzenie część 2](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2.md).

### <a name="adding-a-view-to-the-database"></a>Dodawanie widoku do bazy danych

Na stronie nowe *działy. aspx* , która zostanie utworzona, chcesz podać listę rozwijaną instruktorów z nazwami w formacie "ostatni pierwszy", dzięki czemu użytkownicy mogą wybrać administratorów działu. Aby ułatwić to zadanie, utworzysz widok w bazie danych. Widok będzie zawierać tylko te dane, które są potrzebne przez listę rozwijaną: pełna nazwa (poprawnie sformatowana) i klucz rekordu.

W **Eksplorator serwera**rozwiń pozycję *szkoła. mdf*, kliknij prawym przyciskiem myszy folder **widoki** , a następnie wybierz polecenie **Dodaj nowy widok**.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image13.png)

Kliknij przycisk **Zamknij** po wyświetleniu okna dialogowego **Dodawanie tabeli** i wklej następującą instrukcję SQL do okienka SQL:

[!code-sql[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample1.sql)]

Zapisz widok jako `vInstructorName`.

### <a name="updating-the-data-model"></a>Aktualizowanie modelu danych

W folderze *dal* Otwórz plik *SchoolModel. edmx* , kliknij prawym przyciskiem myszy powierzchnię projektu i wybierz polecenie **Aktualizuj model z bazy danych**.

[![Image07](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image15.png)

W oknie dialogowym **Wybierz obiekty bazy danych** wybierz kartę **Dodaj** i wybierz widok, który właśnie został utworzony.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image17.png)

Kliknij przycisk **Finish** (Zakończ).

W projektancie zobaczysz, że narzędzie utworzyło jednostkę `vInstructorName` i nowe skojarzenie między jednostkami `Department` i `Person`.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image19.png)

> [!NOTE]
> W **danych wyjściowych** i **Lista błędów** może zostać wyświetlony komunikat ostrzegawczy informujący o tym, że narzędzie automatycznie utworzyło klucz podstawowy dla nowego widoku `vInstructorName`. Jest to oczekiwane zachowanie.

Gdy odwołujesz się do nowej jednostki `vInstructorName` w kodzie, nie chcesz używać konwencji bazy danych z prefiksem "v" z małymi literami. W związku z tym należy zmienić nazwę jednostki i zestawu jednostek w modelu.

Otwórz **przeglądarkę modeli**. Zobaczysz `vInstructorName` wymieniony jako typ jednostki i widok.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image21.png)

W obszarze **SchoolModel** (nie **SchoolModel. Store**) kliknij prawym przyciskiem myszy pozycję **vInstructorName** i wybierz pozycję **Właściwości**. W oknie **Właściwości** Zmień właściwość **Nazwa** na "InstructorName" i zmień właściwość Nazwa **zestawu jednostek** na "InstructorNames".

[![Image15](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image24.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image23.png)

Zapisz i Zamknij model danych, a następnie Skompiluj ponownie projekt.

## <a name="using-a-repository-class-and-an-objectdatasource-control"></a>Korzystanie z klasy repozytorium i kontrolki ObjectDataSource

Utwórz nowy plik klasy w folderze *dal* , nadaj mu nazwę *SchoolRepository.cs*i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample2.cs)]

Ten kod zawiera jedną metodę `GetDepartments`, która zwraca wszystkie jednostki z zestawu jednostek `Departments`. Ponieważ wiadomo, że uzyskujesz dostęp do `Person` właściwości nawigacji dla każdego zwróconego wiersza, określisz eager ładowania dla tej właściwości przy użyciu metody `Include`. Klasa implementuje również interfejs `IDisposable`, aby upewnić się, że połączenie z bazą danych jest zwalniane, gdy obiekt zostanie usunięty.

> [!NOTE]
> Typowym sposobem jest utworzenie klasy repozytorium dla każdego typu jednostki. W tym samouczku zostanie użyta jedna Klasa repozytorium dla wielu typów jednostek. Aby uzyskać więcej informacji na temat wzorca repozytorium, zobacz wpisy w [blogu zespołu Entity Framework](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) i [blogu Julie Lerman](http://thedatafarm.com/blog/data-access/agile-ef4-repository-part-3-fine-tuning-the-repository/).

Metoda `GetDepartments` zwraca obiekt `IEnumerable`, a nie obiekt `IQueryable` w celu upewnienia się, że zwracana kolekcja jest użyteczna nawet po usunięciu samego obiektu repozytorium. Obiekt `IQueryable` może spowodować dostęp do bazy danych za każdym razem, gdy zostanie on uzyskany, ale obiekt repozytorium może zostać zlikwidowany przez czas, gdy kontrolka powiązania danych będzie próbować renderować dane. Można zwrócić inny typ kolekcji, taki jak obiekt `IList`, a nie `IEnumerable` obiektu. Jednak zwrócenie obiektu `IEnumerable` gwarantuje, że można wykonywać typowe zadania przetwarzania listy tylko do odczytu, takie jak pętle `foreach` i zapytania LINQ, ale nie można dodawać ani usuwać elementów w kolekcji, co może oznaczać, że takie zmiany byłyby utrwalane w bazie danych.

Utwórz stronę *działS. aspx* korzystającą ze strony wzorcowej *site. Master* i Dodaj następujące znaczniki do kontrolki `Content` o nazwie `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample3.aspx)]

Ten znacznik tworzy formant `ObjectDataSource`, który używa właśnie utworzonej klasy repozytorium, oraz kontrolki `GridView` do wyświetlania danych. Kontrolka `GridView` określa polecenia **edycji** i **usuwania** , ale nie dodano jeszcze kodu do ich obsługi.

Kilka kolumn używa formantów `DynamicField`, aby można było korzystać z funkcji automatycznego formatowania i sprawdzania poprawności danych. Aby te czynności działały, należy wywołać metodę `EnableDynamicData` w obsłudze zdarzeń `Page_Init`. (kontrolki`DynamicControl` nie są używane w polu `Administrator`, ponieważ nie działają z właściwościami nawigacji).

Atrybuty `Vertical-Align="Top"` staną się ważne później, gdy dodasz kolumnę, która ma zagnieżdżoną kontrolkę `GridView` do siatki.

Otwórz plik *Departments.aspx.cs* i Dodaj następującą instrukcję `using`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample4.cs)]

Następnie Dodaj następujący program obsługi dla zdarzenia `Init` strony:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample5.cs)]

W folderze *dal* Utwórz nowy plik klasy o nazwie *Department.cs* i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample6.cs)]

Ten kod dodaje metadane do modelu danych. Określa, że właściwość `Budget` jednostki `Department` faktycznie reprezentuje walutę, chociaż jej typem danych jest `Decimal`i określa, że wartość musi zawierać się w przedziale od 0 do $1 000 000,00. Określa również, że właściwość `StartDate` powinna być sformatowana jako data w formacie mm/dd/rrrr.

Uruchom stronę *działS. aspx* .

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image26.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image25.png)

Zwróć uwagę, że mimo że nie określono ciągu formatu w znacznikach strony *. aspx* dla **wartości budżet** lub **Data rozpoczęcia** , do `DynamicField` nich zastosowano domyślne formatowanie waluty i daty, korzystając z metadanych dostarczonych w pliku *Department.cs* .

## <a name="adding-insert-and-delete-functionality"></a>Dodawanie funkcji INSERT i DELETE

Otwórz *SchoolRepository.cs*, Dodaj następujący kod w celu utworzenia metody `Insert` i metody `Delete`. Kod zawiera również metodę o nazwie `GenerateDepartmentID`, która oblicza kolejną dostępną wartość klucza rekordu do użycia przez metodę `Insert`. Jest to wymagane, ponieważ baza danych nie jest skonfigurowana do automatycznego obliczania dla tabeli `Department`.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample7.cs)]

### <a name="the-attach-method"></a>Attach — Metoda

Metoda `DeleteDepartment` wywołuje metodę `Attach` w celu ponownego nawiązania połączenia, które jest utrzymywane w Menedżerze stanu obiektów kontekstu obiektu między jednostką w pamięci a wierszem bazy danych, który reprezentuje. Musi się to zdarzyć przed wywołaniem metody `SaveChanges` przez metodę.

*Kontekst obiektu* termin odwołuje się do klasy Entity Framework, która pochodzi od klasy `ObjectContext`, która służy do uzyskiwania dostępu do zestawów jednostek i jednostek. W kodzie dla tego projektu Klasa ma nazwę `SchoolEntities`, a wystąpienie ma zawsze nazwę `context`. *Menedżer stanu obiektów* w kontekście obiektu jest klasą pochodzącą od klasy `ObjectStateManager`. Kontakt obiektu używa menedżera stanu obiektów do przechowywania obiektów jednostki i śledzenia, czy każda z nich jest zsynchronizowana z odpowiednim wierszem tabeli lub wierszami w bazie danych.

Podczas odczytywania jednostki, kontekst obiektu zapisuje go w Menedżerze stanu obiektów i śledzi, czy reprezentacja obiektu jest zsynchronizowana z bazą danych. Na przykład jeśli zmienisz wartość właściwości, flaga zostanie ustawiona, aby wskazać, że zmieniona właściwość nie jest już zsynchronizowana z bazą danych. Następnie wywołując metodę `SaveChanges`, kontekst obiektu wie, co należy zrobić w bazie danych, ponieważ Menedżer stanu obiektów wie dokładnie, co się dzieje między bieżącym stanem jednostki a stanem bazy danych.

Jednak ten proces zazwyczaj nie działa w aplikacji sieci Web, ponieważ wystąpienie kontekstu obiektu, które odczytuje jednostkę, wraz ze wszystkimi elementami w Menedżerze stanu obiektów, jest usuwane po wyrenderowaniu strony. Wystąpienie kontekstu obiektu, które musi zastosować zmiany, to nowy, który zostanie utworzony w celu przetworzenia ogłaszania zwrotnego. W przypadku metody `DeleteDepartment`, formant `ObjectDataSource` automatycznie tworzy oryginalną wersję jednostki dla Ciebie z wartości w stanie widoku, ale ta retworzona jednostka `Department` nie istnieje w Menedżerze stanu obiektów. Jeśli wywołano metodę `DeleteObject` dla tej ponownie utworzonej jednostki, wywołanie zakończy się niepowodzeniem, ponieważ kontekst obiektu nie wie, czy jednostka jest zsynchronizowana z bazą danych. Jednak wywołanie metody `Attach` powoduje ponowne ustanowienie tego samego śledzenia między nowo utworzoną jednostką a wartościami w bazie danych, które zostały pierwotnie wykonane automatycznie podczas odczytywania jednostki we wcześniejszym wystąpieniu kontekstu obiektu.

Istnieją przypadki, gdy nie chcesz, aby kontekst obiektu śledził jednostki w Menedżerze stanu obiektów, i możesz ustawić flagi, aby uniemożliwić wykonanie tej czynności. Przykłady tych elementów przedstawiono w kolejnych samouczkach w tej serii.

### <a name="the-savechanges-method"></a>Metoda metody SaveChanges

W tej prostej klasie repozytorium przedstawiono podstawowe zasady wykonywania operacji CRUD. W tym przykładzie metoda `SaveChanges` jest wywoływana natychmiast po każdej aktualizacji. W aplikacji produkcyjnej możesz chcieć wywołać metodę `SaveChanges` z oddzielnej metody, aby zapewnić większą kontrolę nad aktualizacją bazy danych. (Na końcu następnego samouczka znajdziesz link do oficjalnego dokumentu, który omawia jednostkę wzorca pracy, która jest jednym z podejścia do koordynowania powiązanych aktualizacji). Zwróć również uwagę, że w tym przykładzie metoda `DeleteDepartment` nie zawiera kodu do obsługi konfliktów współbieżności; kod do wykonania zostanie dodany w kolejnym samouczku w tej serii.

### <a name="retrieving-instructor-names-to-select-when-inserting"></a>Pobieranie nazw instruktorów do wyboru podczas wstawiania

Podczas tworzenia nowych działów użytkownicy muszą być w stanie wybrać administratora z listy rozwijanej. W związku z tym Dodaj następujący kod do *SchoolRepository.cs* , aby utworzyć metodę pobierania listy instruktorów przy użyciu utworzonego wcześniej widoku:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample8.cs)]

### <a name="creating-a-page-for-inserting-departments"></a>Tworzenie strony do wstawiania działów

Utwórz stronę *DepartmentsAdd. aspx* korzystającą ze strony *site. Master* i Dodaj następujące znaczniki w kontrolce `Content` o nazwie `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample9.aspx)]

Ten znacznik tworzy dwie `ObjectDataSource` kontrolki, jeden do wstawiania nowych jednostek `Department` i jeden do pobierania nazw instruktorów dla kontrolki `DropDownList`, który jest używany do wybierania administratorów działu. Znacznik tworzy `DetailsView` kontrolkę do wprowadzania nowych działów i określa procedurę obsługi dla zdarzenia `ItemInserting` kontrolki, aby można było ustawić `Administrator` wartość klucza obcego. Na końcu jest formantem `ValidationSummary`, aby wyświetlić komunikaty o błędach.

Otwórz *DepartmentsAdd.aspx.cs* i Dodaj następującą instrukcję `using`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample10.cs)]

Dodaj następującą zmienną klasy i metody:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample11.cs)]

Metoda `Page_Init` umożliwia korzystanie z funkcji danych dynamicznych. Program obsługi zdarzenia `Init` formantu `DropDownList` zapisuje odwołanie do kontrolki, a program obsługi dla zdarzenia `Inserting` kontrolki `DetailsView` używa tego odwołania, aby uzyskać `PersonID` wartość wybranego instruktora i zaktualizować Właściwość `Administrator` klucza obcego jednostki `Department`.

Uruchom stronę, Dodaj informacje dla nowego działu, a następnie kliknij link **Wstaw** .

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image28.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image27.png)

Wprowadź wartości dla innego nowego działu. Wprowadź liczbę większą niż 1 000 000,00 w polu **budżetu** i Tab do następnego pola. W polu zostanie wyświetlona gwiazdka, a po umieszczeniu nad nim wskaźnika myszy zobaczysz komunikat o błędzie wprowadzony w metadanych dla tego pola.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image30.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image29.png)

Kliknij przycisk **Wstaw**, a zobaczysz komunikat o błędzie wyświetlany przez kontrolkę `ValidationSummary` w dolnej części strony.

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image32.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image31.png)

Następnie zamknij przeglądarkę i Otwórz stronę *działS. aspx* . Dodaj możliwość usuwania do strony *działS. aspx* , dodając atrybut `DeleteMethod` do kontrolki `ObjectDataSource` i atrybut `DataKeyNames` do kontrolki `GridView`. Tagi otwierające tych kontrolek będą teraz podobne do następujących:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample12.aspx)]

Uruchom stronę.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image34.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image33.png)

Usunięcie działu dodanego podczas uruchamiania strony *DepartmentsAdd. aspx* .

## <a name="adding-update-functionality"></a>Dodawanie funkcji aktualizacji

Otwórz *SchoolRepository.cs* i Dodaj następującą metodę `Update`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample13.cs)]

Po kliknięciu przycisku **Aktualizuj** na stronie *działS. aspx* formant `ObjectDataSource` tworzy dwie jednostki `Department` do przekazania do metody `UpdateDepartment`. Jeden zawiera oryginalne wartości, które były przechowywane w stanie widoku, a drugi zawiera nowe wartości wprowadzone w kontrolce `GridView`. Kod w metodzie `UpdateDepartment` przekazuje jednostkę `Department`, która ma pierwotne wartości do metody `Attach` w celu ustalenia śledzenia między jednostką a wartością znajdującą się w bazie danych. Następnie kod przekazuje jednostkę `Department`, która ma nowe wartości do metody `ApplyCurrentValues`. Kontekst obiektu zawiera porównanie starych i nowych wartości. Jeśli nowa wartość różni się od starej wartości, kontekst obiektu zmienia wartość właściwości. Następnie Metoda `SaveChanges` aktualizuje tylko zmienione kolumny w bazie danych. Jeśli jednak funkcja Update dla tej jednostki została zmapowana do procedury składowanej, cały wiersz zostanie zaktualizowany niezależnie od tego, które kolumny zostały zmienione.

Otwórz plik *działS. aspx* i Dodaj następujące atrybuty do kontrolki `DepartmentsObjectDataSource`:

- `UpdateMethod="UpdateDepartment"`
- `ConflictDetection="CompareAllValues"`   
 Powoduje to, że stare wartości są przechowywane w stanie widoku, dzięki czemu można je porównać z nowymi wartościami w metodzie `Update`.
- `OldValuesParameterFormatString="orig{0}"`   
 Informuje o tym, że nazwa parametru oryginalnych wartości jest `origDepartment`.

Znaczniki dla tagu otwierającego kontrolki `ObjectDataSource` teraz przypominają następujący przykład:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample14.aspx)]

Dodaj atrybut `OnRowUpdating="DepartmentsGridView_RowUpdating"` do kontrolki `GridView`. Służy do ustawiania wartości właściwości `Administrator` na podstawie wiersza wybieranego przez użytkownika na liście rozwijanej. `GridView` tagu otwierającego jest teraz podobna do poniższego przykładu:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample15.aspx)]

Dodaj kontrolkę `EditItemTemplate` dla kolumny `Administrator` do kontrolki `GridView` bezpośrednio po kontrolce `ItemTemplate` dla tej kolumny:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample16.aspx)]

Ten formant `EditItemTemplate` jest podobny do kontrolki `InsertItemTemplate` na stronie *DepartmentsAdd. aspx* . Różnica polega na tym, że początkowa wartość kontrolki jest ustawiana za pomocą atrybutu `SelectedValue`.

Przed kontrolką `GridView` Dodaj kontrolkę `ValidationSummary`, tak jak na stronie *DepartmentsAdd. aspx* .

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample17.aspx)]

Otwórz *Departments.aspx.cs* i bezpośrednio po deklaracji klasy częściowej Dodaj następujący kod, aby utworzyć pole prywatne do odwoływania się do formantu `DropDownList`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample18.cs)]

Następnie Dodaj programy obsługi dla zdarzenia `Init` kontrolki `DropDownList` i zdarzenia `RowUpdating` kontrolki `GridView`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample19.cs)]

Procedura obsługi dla zdarzenia `Init` zapisuje odwołanie do kontrolki `DropDownList` w polu Class. Procedura obsługi dla zdarzenia `RowUpdating` używa odwołania, aby pobrać wartość wprowadzoną przez użytkownika i zastosować ją do właściwości `Administrator` jednostki `Department`.

Użyj strony *DepartmentsAdd. aspx* , aby dodać nowy dział, a następnie Uruchom stronę *działS. aspx* i kliknij pozycję **Edytuj** w wierszu, który został dodany.

> [!NOTE]
> Nie będzie można edytować wierszy, które nie zostały dodane (to jest już w bazie danych) z powodu nieprawidłowych danych w bazie danych; Administratorzy dla wierszy, które zostały utworzone w bazie danych, są uczniami. Jeśli spróbujesz edytować jeden z nich, otrzymasz stronę błędu, która zgłosi błąd, taki jak `'InstructorsDropDownList' has a SelectedValue which is invalid because it does not exist in the list of items.`

[![Image10](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image36.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image35.png)

Jeśli wprowadzisz nieprawidłową kwotę **budżetu** , a następnie klikniesz przycisk **Aktualizuj**, zobaczysz tę samą gwiazdkę i komunikat o błędzie, który został wyświetlony na stronie *działS. aspx* .

Zmień wartość pola lub wybierz innego administratora, a następnie kliknij przycisk **Aktualizuj**. Zostanie wyświetlona zmiana.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image38.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image37.png)

W tym celu wprowadzamy wprowadzenie do używania kontrolki `ObjectDataSource` dla podstawowych operacji CRUD (tworzenie, odczytywanie, aktualizowanie i usuwanie) z Entity Framework. Wbudowana jest prosta aplikacja n-warstwowa, ale Warstwa logiki biznesowej jest nadal ściśle sprzężona z warstwą dostępu do danych, co komplikuje automatyczne testowanie jednostek. W poniższym samouczku przedstawiono sposób implementacji wzorca repozytorium w celu ułatwienia testów jednostkowych.

> [!div class="step-by-step"]
> [Dalej](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)

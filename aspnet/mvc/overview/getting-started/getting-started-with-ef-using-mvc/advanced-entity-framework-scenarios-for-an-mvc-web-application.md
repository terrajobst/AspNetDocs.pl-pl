---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: 'Samouczek: Dowiedz się więcej na temat zaawansowanych scenariuszy EF dla aplikacji sieci Web MVC 5'
description: Ten samouczek zawiera wprowadzenie do kilku tematów, które są przydatne, gdy wykraczasz poza podstawowe informacje na temat tworzenia aplikacji sieci Web ASP.NET korzystających z Entity Framework Code First.
author: tdykstra
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: f35a9b0c-49ef-4cde-b06d-19d1543feb0b
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: d7cc83a5b78a60f575f5c3065079679189296a0c
ms.sourcegitcommit: 4b324a11131e38f920126066b94ff478aa9927f8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/16/2019
ms.locfileid: "58425278"
---
# <a name="tutorial-learn-about-advanced-ef-scenarios-for-an-mvc-5-web-app"></a>Samouczek: Dowiedz się więcej na temat zaawansowanych scenariuszy EF dla aplikacji sieci Web MVC 5

W poprzednim samouczku zaimplementowano dziedziczenie w hierarchii na poziomie tabeli. Ten samouczek zawiera wprowadzenie do kilku tematów, które są przydatne, gdy wykraczasz poza podstawowe informacje na temat tworzenia aplikacji sieci Web ASP.NET korzystających z Entity Framework Code First. W kilku pierwszych sekcjach przedstawiono instrukcje krok po kroku, które przeprowadzą Cię przez kod i używając programu Visual Studio do wykonania zadań, w których kolejne sekcje zawierają kilka tematów z krótkimi wprowadzeniem, a następnie linki do zasobów, aby uzyskać więcej informacji.

W przypadku większości tych tematów będziesz korzystać ze stron, które zostały już utworzone. Aby użyć nieprzetworzonej bazy danych SQL do aktualizacji zbiorczych, należy utworzyć nową stronę, która aktualizuje liczbę kredytów wszystkich kursów w bazie danych:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

W tym samouczku przedstawiono następujące instrukcje:

> [!div class="checklist"]
> * Wykonywanie nieprzetworzonych zapytań SQL
> * Wykonywanie zapytań bez śledzenia
> * Sprawdzanie zapytań SQL wysyłanych do bazy danych

Poznasz również następujące informacje:

> [!div class="checklist"]
> * Tworzenie warstwy abstrakcji
> * Klasy proxy
> * Automatyczne wykrywanie zmian
> * Automatyczna walidacja
> * Entity Framework narzędzia do zarządzania
> * Entity Framework kod źródłowy

## <a name="prerequisite"></a>Wymaganie wstępne

* [Implementowanie dziedziczenia](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="perform-raw-sql-queries"></a>Wykonywanie nieprzetworzonych zapytań SQL

Interfejs API Code First Entity Framework zawiera metody umożliwiające przekazywanie poleceń SQL bezpośrednio do bazy danych. Do wyboru są następujące opcje:

- Użyj metody [nieogólnymi. sqlQuery](https://msdn.microsoft.com/library/system.data.entity.dbset.sqlquery.aspx) dla zapytań, które zwracają typy jednostek. Zwracane obiekty muszą być typu oczekiwanego przez `DbSet` obiekt i są automatycznie śledzone przez kontekst bazy danych, chyba że zostanie wyłączone śledzenie. (Zobacz poniższą sekcję dotyczącą metody [AsNoTracking](https://msdn.microsoft.com/library/system.data.entity.dbextensions.asnotracking.aspx) ).
- Użyj metody [Database. sqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery.aspx) dla zapytań, które zwracają typy, które nie są jednostkami. Zwrócone dane nie są śledzone przez kontekst bazy danych, nawet jeśli ta metoda jest używana do pobierania typów jednostek.
- Użyj elementu [Database. ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456.aspx) dla poleceń niezwiązanych z kwerendą.

Jedną z zalet korzystania z Entity Framework jest to, że pozwala to uniknąć zbyt ścisłego powiązana kodu z konkretną metodą przechowywania danych. Robi to przez generowanie zapytań i poleceń SQL, które również uwalniają użytkownika od konieczności pisania. Ale istnieją wyjątkowe scenariusze, które należy wykonać w celu uruchomienia określonych zapytań SQL, które zostały utworzone ręcznie. te metody umożliwiają obsługę takich wyjątków.

Gdy jest zawsze prawdziwe w przypadku wykonywania poleceń SQL w aplikacji sieci Web, należy podjąć środki ostrożności, aby chronić witrynę przed atakami polegającymi na iniekcji SQL. Jednym ze sposobów jest użycie zapytań parametrycznych w celu upewnienia się, że ciągi przesłane przez stronę sieci Web nie mogą być interpretowane jako polecenia SQL. W tym samouczku użyjesz zapytań parametrycznych podczas integrowania danych wejściowych użytkownika z zapytaniem.

### <a name="calling-a-query-that-returns-entities"></a>Wywoływanie zapytania zwracającego jednostki

Klasa [nieogólnymi&lt;zamiaru&gt; ](https://msdn.microsoft.com/library/gg696460.aspx) udostępnia metodę, której można użyć do wykonania zapytania zwracającego jednostkę typu. `TEntity` Aby zobaczyć, jak to działa, Zmień kod w `Details` metodzie `Department` kontrolera.

W *DepartmentController.cs*, w `Details` `db.Departments.FindAsync` metodzie, Zastąp wywołanie `db.Departments.SqlQuery` metody wywołaniem metody, jak pokazano w poniższym wyróżnionym kodzie:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs?highlight=8-14)]

Aby sprawdzić, czy nowy kod działa prawidłowo, wybierz kartę **działy** , a następnie **szczegóły** dla jednego z działów. Upewnij się, że wszystkie dane są wyświetlane zgodnie z oczekiwaniami.

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>Wywoływanie zapytania zwracającego inne typy obiektów

Wcześniej utworzono siatkę statystyk uczniów dla strony informacje, która wykazała liczbę studentów dla każdej daty rejestracji. Kod, który robi ten w *HomeController.cs* używa LINQ:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

Załóżmy, że chcesz napisać kod pobierający te dane bezpośrednio w języku SQL, a nie za pomocą LINQ. W tym celu należy uruchomić zapytanie, które zwraca coś innego niż obiekty jednostki, co oznacza, że należy użyć metody [Database. sqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery(v=VS.103).aspx) .

W *HomeController.cs*Zastąp instrukcję LINQ w `About` metodzie instrukcją SQL, jak pokazano w następującym wyróżnionym kodzie:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs?highlight=3-18)]

Uruchom stronę informacje. Sprawdź, czy są wyświetlane te same dane, które wcześniej istniały.

### <a name="calling-an-update-query"></a>Wywoływanie zapytania Update

Załóżmy, że administratorzy uniwersytetów firmy Contoso chcą mieć możliwość wykonywania zbiorczych zmian w bazie danych, takich jak zmiana liczby kredytów każdego kursu. Jeśli University ma dużą liczbę kursów, może być nieefektywny do pobrania ich wszystkie jako jednostki i indywidualnie zmienić. W tej sekcji zostanie zaimplementowana Strona sieci Web, która umożliwia użytkownikowi określenie współczynnika, przez który należy zmienić liczbę kredytów dla wszystkich kursów i wprowadzić zmiany przez wykonanie instrukcji SQL `UPDATE` . 

W *CourseController.cs*, Dodaj `UpdateCourseCredits` metody dla `HttpGet` i `HttpPost`:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

Gdy kontroler przetwarza `HttpGet` żądanie, nic nie jest zwracane `ViewBag.RowsAffected` w zmiennej, a widok wyświetla puste pole tekstowe i przycisk Prześlij.

Po kliknięciu `HttpPost` przycisku Aktualizuj Metoda jest wywoływana i `multiplier` ma wartość wprowadzoną w polu tekstowym. Następnie kod wykonuje instrukcję SQL, która aktualizuje kursy i zwraca liczbę odnośnych wierszy do widoku w `ViewBag.RowsAffected` zmiennej. Gdy widok pobiera wartość w tej zmiennej, wyświetla liczbę zaktualizowanych wierszy zamiast pola tekstowego i Prześlij.

W *CourseController.cs*, kliknij prawym przyciskiem myszy jedną `UpdateCourseCredits` z metod, a następnie kliknij polecenie **Dodaj widok**. Zostanie wyświetlone okno dialogowe **Dodawanie widoku** . Pozostaw wartości domyślne i wybierz pozycję **Dodaj**.

W *Views\Course\UpdateCourseCredits.cshtml*Zastąp kod szablonu następującym kodem:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cshtml)]

Uruchom metodę, wybierając kartę **kursy** , a następnie dodając wartość "/UpdateCourseCredits" na końcu adresu URL na pasku adresu przeglądarki (na przykład: `http://localhost:50205/Course/UpdateCourseCredits`). `UpdateCourseCredits` Wprowadź liczbę w polu tekstowym:

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

Kliknij przycisk **Aktualizuj**. Zostanie wyświetlona liczba wierszy, których to dotyczy.

Kliknij przycisk **Wstecz, aby** wyświetlić listę kursów o zmienionej liczbie kredytów.

Aby uzyskać więcej informacji na temat nieprzetworzonych zapytań SQL, zobacz [RAW zapytania SQL](https://msdn.microsoft.com/data/jj592907) w witrynie MSDN.

## <a name="no-tracking-queries"></a>Zapytania bez śledzenia

Gdy kontekst bazy danych pobiera wiersze tabeli i tworzy obiekty jednostek, które reprezentują je, domyślnie śledzi, czy jednostki w pamięci są zsynchronizowane z danymi znajdującymi się w bazie danych. Dane w pamięci pełnią rolę pamięci podręcznej i są używane podczas aktualizowania jednostki. Ta pamięć podręczna jest często niepotrzebna w aplikacji sieci Web, ponieważ wystąpienia kontekstu są zwykle krótkotrwałe (nowy element jest tworzony i usuwany dla każdego żądania), a kontekst, który odczytuje jednostkę, jest zazwyczaj likwidowany przed ponownym użyciem tej jednostki.

Można wyłączyć śledzenie obiektów jednostki w pamięci za pomocą metody [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) . Typowe scenariusze, w których warto wykonać następujące czynności:

- Zapytanie pobiera takie duże ilości danych, które wyłączenie śledzenia może znacznie zwiększyć wydajność.
- Chcesz dołączyć jednostkę, aby ją zaktualizować, ale wcześniej pobrano tę samą jednostkę do innego celu. Ponieważ jednostka jest już śledzona przez kontekst bazy danych, nie można dołączyć jednostki, która ma zostać zmieniona. Jednym ze sposobów obsługi tej sytuacji jest użycie `AsNoTracking` opcji z wcześniejszym zapytaniem.

Aby zapoznać się z przykładem, który ilustruje sposób używania metody [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) , zobacz [wcześniejszą wersję tego samouczka](../../older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Ta wersja samouczka nie ustawia flagi zmodyfikowano w jednostce modelu z modelem spinacza w metodzie Edit, więc nie jest potrzebna `AsNoTracking`.

## <a name="examine-sql-sent-to-database"></a>Sprawdzanie, czy program SQL został wysłany do bazy danych

Czasami warto zobaczyć rzeczywiste zapytania SQL, które są wysyłane do bazy danych. W poprzednim samouczku pokazano, jak to zrobić w kodzie interceptora; teraz zobaczysz kilka sposobów, aby to zrobić bez pisania kodu interceptora. Aby wypróbować ten problem, należy zapoznać się z prostym zapytaniem, a następnie sprawdzić, co się dzieje, w miarę dodawania opcji, takich jak eager ładowanie, filtrowanie i sortowanie.

W obszarze *Kontrolery/CourseController*Zastąp `Index` metodę następującym kodem, aby tymczasowo zatrzymać ładowanie eager:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

Teraz ustaw punkt przerwania w `return` instrukcji (F9 z kursorem w tym wierszu). Naciśnij klawisz **F5** , aby uruchomić projekt w trybie debugowania, a następnie wybierz stronę indeks kursu. Gdy kod osiągnie punkt przerwania, należy przeanalizować `sql` zmienną. Zostanie wyświetlone zapytanie wysyłane do SQL Server. Jest to prosta `Select` instrukcja.

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.json)]

Kliknij szklaną lupę, aby zobaczyć zapytanie w **wizualizatorze tekstu**.

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

Teraz dodasz listę rozwijaną do strony indeks kursów, aby umożliwić użytkownikom filtrowanie określonych działów. Posortujesz kursy według tytułu i określisz eager ładowania dla `Department` właściwości nawigacji.

W *CourseController.cs*Zastąp `Index` metodę następującym kodem:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

Przywróć punkt przerwania w `return` instrukcji.

Metoda odbiera wybraną wartość z listy rozwijanej w `SelectedDepartment` parametrze. Jeśli nic nie jest zaznaczone, ten parametr będzie miał wartość null.

`SelectList` Kolekcja zawierająca wszystkie działy jest przenoszona do widoku listy rozwijanej. Parametry przesłane do `SelectList` konstruktora określają nazwę pola wartości, nazwę pola tekstowego i wybrany element.

`Get` Dla metody `Course` repozytorium kod określa wyrażenie filtru, porządek sortowania `Department` i eager ładowania dla właściwości nawigacji. Wyrażenie filtru zawsze zwraca wartość `true` , jeśli nic nie jest zaznaczone na liście rozwijanej (oznacza to, `SelectedDepartment` że wartość jest równa null).

W *Views\Course\Index.cshtml*, bezpośrednio przed otwierającym `table` tagiem, Dodaj następujący kod, aby utworzyć listę rozwijaną i przycisk Prześlij:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

Gdy punkt przerwania jest nadal ustawiony, Uruchom stronę indeks kursu. Kontynuuj za pierwszym razem, gdy kod trafi punkt przerwania, tak aby strona była wyświetlana w przeglądarce. Wybierz dział z listy rozwijanej i kliknij przycisk **Filtruj**.

Tym razem pierwszy punkt przerwania będzie przeznaczony dla kwerendy działy dla listy rozwijanej. Pomiń i Wyświetl `query` zmienną przy następnym przejściu kodu do punktu przerwania, aby zobaczyć `Course` , jak wygląda zapytanie. Zobaczysz coś podobne do następujących:

[!code-sql[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.sql)]

Można zobaczyć, że zapytanie jest `JOIN` teraz kwerendą, która ładuje `Department` dane wraz z `Course` danymi i zawiera `WHERE` klauzulę.

`var sql = courses.ToString()` Usuń wiersz.

## <a name="create-an-abstraction-layer"></a>Tworzenie warstwy abstrakcji

Wielu deweloperów pisze kod, aby zaimplementować wzorce repozytorium i jednostki pracy jako otokę otaczającą kod, który działa z Entity Framework. Wzorce te mają na celu utworzenie warstwy abstrakcji między warstwą dostępu do danych i warstwą logiki biznesowej aplikacji. Wdrożenie tych wzorców może pomóc w izolowaniu aplikacji od zmian w magazynie danych oraz w celu ułatwienia zautomatyzowanych testów jednostkowych lub projektowania opartego na testach (TDD). Jednak zapisanie dodatkowego kodu w celu zaimplementowania tych wzorców nie zawsze jest najlepszym wyborem dla aplikacji korzystających z EF z kilku powodów:

- Sama klasa kontekstu EF izolowana od kodu z kodu specyficznego dla magazynu danych.
- Klasa kontekstu EF może działać jako Klasa jednostki pracy dla aktualizacji bazy danych korzystających z EF.
- Funkcje wprowadzone w Entity Framework 6 ułatwiają zaimplementowanie TDD bez pisania kodu repozytorium.

Aby uzyskać więcej informacji na temat implementowania wzorców repozytorium i jednostki pracy, zobacz [wersję Entity Framework 5 tej serii samouczków](../../older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md). Aby uzyskać informacje na temat sposobów implementacji usługi TDD w Entity Framework 6, zobacz następujące zasoby:

- [Jak EF6 umożliwia łatwiejsze tworzenie zestawów dbsets](http://thedatafarm.com/data-access/how-ef6-enables-mocking-dbsets-more-easily/)
- [Testowanie przy użyciu struktury imitacji](https://msdn.microsoft.com/data/dn314429)
- [Testowanie przy użyciu własnych testów zostanie podwojone](https://msdn.microsoft.com/data/dn314431)

<a id="proxies"></a>

## <a name="proxy-classes"></a>Klasy proxy

Gdy Entity Framework tworzy wystąpienia jednostek (na przykład podczas wykonywania zapytania), często tworzy je jako wystąpienia dynamicznie generowanego typu pochodnego, który działa jako serwer proxy dla jednostki. Na przykład zapoznaj się z poniższymi dwoma obrazami debugera. Na pierwszym obrazie widać, że `student` zmienna jest oczekiwanym `Student` typem natychmiast po utworzeniu wystąpienia jednostki. W drugim obrazie, po zastosowaniu EF do odczytania jednostki ucznia z bazy danych, zostanie wyświetlona Klasa proxy.

![Przed klasą proxy](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

![Po klasie proxy](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

Ta klasa proxy zastępuje pewne właściwości wirtualne jednostki, aby wstawić punkty zaczepienia do wykonywania akcji automatycznie podczas uzyskiwania dostępu do właściwości. Jedna funkcja tego mechanizmu jest używana w przypadku ładowania z opóźnieniem.

Większość czasu nie musisz wiedzieć o tym, jak korzystać z serwerów proxy, ale istnieją wyjątki:

- W niektórych scenariuszach może być konieczne uniemożliwienie Entity Framework tworzenia wystąpień serwera proxy. Na przykład podczas serializowania jednostek zwykle potrzebne są klasy POCO, a nie klasy proxy. Jednym ze sposobów uniknięcia problemów serializacji jest Serializowanie obiektów transferu danych (DTO) zamiast obiektów jednostek, jak pokazano w samouczku [using Web API with Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-1.md) . Innym sposobem jest [wyłączenie tworzenia serwera proxy](https://msdn.microsoft.com/data/jj592886.aspx).
- Podczas tworzenia wystąpienia klasy jednostki przy użyciu `new` operatora nie jest możliwe uzyskanie wystąpienia serwera proxy. Oznacza to, że nie otrzymujesz takich funkcji, jak ładowanie z opóźnieniem i automatyczne śledzenie zmian. Jest to zazwyczaj dobry; zwykle nie jest wymagane ładowanie z opóźnieniem, ponieważ tworzysz nową jednostkę, która nie znajduje się w bazie danych, a na ogół nie potrzebujesz śledzenia zmian, jeśli jawnie oznaczesz jednostkę `Added`jako. Jeśli jednak wymagasz ładowania z opóźnieniem i potrzebujesz śledzenia zmian, możesz utworzyć nowe wystąpienia jednostek z serwerami proxy za pomocą metody `DbSet` [Create](https://msdn.microsoft.com/library/gg679504.aspx) klasy.
- Możesz chcieć uzyskać rzeczywisty typ jednostki z typu proxy. Można użyć metody [GetObjectType](https://msdn.microsoft.com/library/system.data.objects.objectcontext.getobjecttype.aspx) klasy, `ObjectContext` Aby uzyskać rzeczywisty typ jednostki wystąpienia typu serwera proxy.

Aby uzyskać więcej informacji, zobacz [Praca z serwerami proxy](https://msdn.microsoft.com/data/JJ592886.aspx) w witrynie MSDN.

## <a name="automatic-change-detection"></a>Automatyczne wykrywanie zmian

Entity Framework określa, w jaki sposób jednostka została zmieniona (i w związku z tym, które aktualizacje muszą być wysyłane do bazy danych), porównując bieżące wartości jednostki z oryginalnymi wartościami. Oryginalne wartości są przechowywane, gdy obiekt jest wysyłany lub dołączany. Niektóre metody, które powodują automatyczne wykrywanie zmian, są następujące:

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

Jeśli śledzisz dużą liczbę jednostek i wywołujesz jedną z tych metod wiele razy w pętli, możesz uzyskać znaczące ulepszenia wydajności, tymczasowo wyłączając automatyczne wykrywanie zmian za pomocą właściwości [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled.aspx) . Aby uzyskać więcej informacji, zobacz [Automatyczne wykrywanie zmian](https://msdn.microsoft.com/data/jj556205) w witrynie MSDN.

## <a name="automatic-validation"></a>Automatyczna walidacja

Po wywołaniu `SaveChanges` metody, domyślnie Entity Framework sprawdza poprawność danych we wszystkich właściwościach wszystkich zmienionych jednostek przed aktualizacją bazy danych. Jeśli Zaktualizowano dużą liczbę jednostek i masz już zweryfikowane dane, ta wartość nie jest konieczna, a proces zapisywania zmian trwa krócej, tymczasowo wyłączając weryfikację. Można to zrobić za pomocą właściwości [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled.aspx) . Aby uzyskać więcej informacji, zobacz [Walidacja](https://msdn.microsoft.com/data/gg193959) w witrynie MSDN.

## <a name="entity-framework-power-tools"></a>Entity Framework narzędzia do zarządzania

[Entity Framework PowerShell](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) to dodatek programu Visual Studio, który został użyty do utworzenia diagramów modelu danych pokazanych w tych samouczkach. Narzędzia mogą również wykonywać inne funkcje, takie jak generowanie klas jednostek na podstawie tabel w istniejącej bazie danych, dzięki czemu można użyć bazy danych z Code First. Po zainstalowaniu narzędzi niektóre dodatkowe opcje są wyświetlane w menu kontekstowym. Na przykład po kliknięciu prawym przyciskiem myszy klasy kontekstowej w **Eksplorator rozwiązań**zostanie wyświetlona opcja i **Entity Framework** . Daje to możliwość wygenerowania diagramu. Gdy używasz Code First nie możesz zmienić modelu danych na diagramie, ale możesz przenieść elementy, aby ułatwić zrozumienie.

![Diagram EF](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image15.png)

## <a name="entity-framework-source-code"></a>Entity Framework kod źródłowy

Kod źródłowy dla Entity Framework 6 jest dostępny w witrynie [GitHub](https://github.com/aspnet/EntityFramework6). Można tworzyć pliki błędów i tworzyć własne rozszerzenia kodu źródłowego EF.

Chociaż kod źródłowy jest otwarty, Entity Framework jest w pełni obsługiwany jako produkt firmy Microsoft. Zespół Entity Framework firmy Microsoft zachowuje kontrolę nad tym, jakie wkłady są akceptowane i sprawdza wszystkie zmiany kodu w celu zapewnienia jakości każdej wersji.

## <a name="acknowledgments"></a>Potwierdzeń

- Tomasz Dykstra zapisał oryginalną wersję tego samouczka, współutworzył aktualizację EF 5 i zapisała aktualizację EF 6. Tomasz to starszy moduł zapisujący Programowanie na platformie sieci Web firmy Microsoft i narzędzi zespołu ds. zawartości.
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (Twitter [@RickAndMSFT](http://twitter.com/RickAndMSFT)) przeszedł większość pracy z aktualizacją samouczka dla programu Ef 5 i MVC 4 i współautorem aktualizacji Dr 6. Rick to starszy moduł zapisujący Programowanie dla firmy Microsoft skupiając się na platformie Azure i MVC.
- [Rowan Miller](http://www.romiller.com) i inni członkowie zespołu Entity Framework mogą korzystać z przeglądów kodu i pomóc w debugowaniu wielu problemów dotyczących migracji, które powstały podczas aktualizowania samouczka dotyczącego programu Ef 5 i EF 6.

## <a name="troubleshoot-common-errors"></a>Rozwiązywanie typowych błędów

### <a name="cannot-createshadow-copy"></a>Nie można utworzyć/skopiować w tle

Komunikat o błędzie:

> Nie można utworzyć/skopiować&lt;&gt;pliku w tle, gdy ten plik już istnieje.

Rozwiązanie

Odczekaj kilka sekund i Odśwież stronę.

### <a name="update-database-not-recognized"></a>Aktualizacja — nie rozpoznano bazy danych

Komunikat o błędzie (z `Update-Database` polecenia w obszarze PMC):

> Termin "Update-Database" nie jest rozpoznawany jako nazwa polecenia cmdlet, funkcji, pliku skryptu ani programu interoperacyjnego. Sprawdź pisownię nazwy lub, jeśli ścieżka została uwzględniona, sprawdź, czy ścieżka jest poprawna, i spróbuj ponownie.

Rozwiązanie

Zamknij program Visual Studio. Otwórz ponownie projekt i ponów próbę.

### <a name="validation-failed"></a>Weryfikacja nie powiodła się

Komunikat o błędzie (z `Update-Database` polecenia w obszarze PMC):

> Walidacja nie powiodła się dla co najmniej jednej jednostki. Aby uzyskać więcej informacji, zobacz Właściwość "EntityValidationErrors".

Rozwiązanie

Jedną z przyczyn tego problemu jest błędy walidacji podczas `Seed` uruchamiania metody. Zapoznaj się `Seed` z [wypełnieniem i debugowaniem Entity Framework (EF) baz danych](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) , aby uzyskać wskazówki dotyczące debugowania metody.

### <a name="http-50019-error"></a>Błąd HTTP 500,19

Komunikat o błędzie:

> Błąd HTTP 500,19 — wewnętrzny błąd serwera nie można uzyskać dostępu do żądanej strony, ponieważ powiązane dane konfiguracji strony są nieprawidłowe.

Rozwiązanie

Jednym ze sposobów uzyskania tego błędu jest posiadanie wielu kopii rozwiązania, z których każdy korzysta z tego samego numeru portu. Zazwyczaj można rozwiązać ten problem, opuszczając wszystkie wystąpienia programu Visual Studio, a następnie uruchamiając ponownie projekt, nad którym pracujesz. Jeśli to nie zadziała, spróbuj zmienić numer portu. Kliknij prawym przyciskiem myszy plik projektu, a następnie kliknij polecenie Właściwości. Wybierz kartę **Sieć Web** , a następnie Zmień numer portu w polu tekstowym **adres URL projektu** .

### <a name="error-locating-sql-server-instance"></a>Błąd lokalizowania wystąpienia SQL Server

Komunikat o błędzie:

> Wystąpił błąd związany z siecią lub wystąpieniem podczas ustanawiania połączenia z SQL Server. Serwer nie został odnaleziony lub był niedostępny. Sprawdź, czy nazwa wystąpienia jest poprawna i czy SQL Server jest skonfigurowany do zezwalania na połączenia zdalne. dostawcy Interfejsy sieciowe SQL, błąd: 26-błąd podczas lokalizowania określonego serwera/wystąpienia)

Rozwiązanie

Sprawdź parametry połączenia. Jeśli baza danych została ręcznie usunięta, Zmień nazwę bazy danych w ciągu konstrukcji.

## <a name="get-the-code"></a>Pobierz kod

[Pobierz ukończony projekt](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Dodatkowe zasoby

 Aby uzyskać więcej informacji na temat sposobu pracy z danymi przy użyciu Entity Framework, zobacz [stronę dokumentacji EF w witrynie MSDN](https://msdn.microsoft.com/data/ee712907) i [ASP.NET dostęp do danych — zalecane zasoby](../../../../whitepapers/aspnet-data-access-content-map.md).

Aby uzyskać więcej informacji na temat sposobu wdrażania aplikacji sieci Web po jej skompilowaniu, zobacz artykuł [ASP.NET Web Deployment — zalecane zasoby](../../../../whitepapers/aspnet-web-deployment-content-map.md) w bibliotece MSDN.

Aby uzyskać informacje dotyczące innych tematów związanych z MVC, takich jak uwierzytelnianie i autoryzacja, zobacz [zasoby zalecane przez ASP.NET MVC](../recommended-resources-for-mvc.md).

## <a name="next-steps"></a>Następne kroki

W tym samouczku przedstawiono następujące instrukcje:

> [!div class="checklist"]
> * Wykonywanie nieprzetworzonych zapytań SQL
> * Wykonywanie zapytań bez śledzenia
> * Zbadano zapytania SQL wysłane do bazy danych

Poznasz również następujące informacje:

> [!div class="checklist"]
> * Tworzenie warstwy abstrakcji
> * Klasy proxy
> * Automatyczne wykrywanie zmian
> * Automatyczna walidacja
> * Entity Framework narzędzia do zarządzania
> * Entity Framework kod źródłowy

Ta seria samouczków została zakończona przy użyciu Entity Framework w aplikacji ASP.NET MVC. Jeśli chcesz dowiedzieć się więcej na temat EF Database First, zobacz pierwsza seria samouczków bazy danych.
> [!div class="nextstepaction"]
> [Entity Framework Database First](../database-first-development/setting-up-database.md)
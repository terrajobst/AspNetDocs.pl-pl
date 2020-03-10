---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
title: Implementacja wzorców repozytorium i jednostki pracy w aplikacji ASP.NET MVC (9 z 10) | Microsoft Docs
author: tdykstra
description: Przykładowa aplikacja internetowa Contoso University pokazuje, jak tworzyć aplikacje ASP.NET MVC 4 przy użyciu Code First Entity Framework 5 i programu Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 44761193-04ba-4990-9f90-145d3c10a716
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 18de9b125ee5d10795b9ce1a366918dadf4fc4e3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78540265"
---
# <a name="implementing-the-repository-and-unit-of-work-patterns-in-an-aspnet-mvc-application-9-of-10"></a>Implementacja wzorców repozytorium i jednostki pracy w aplikacji ASP.NET MVC (9 z 10)

Autor [Dykstra](https://github.com/tdykstra)

[Pobierz ukończony projekt](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Przykładowa aplikacja internetowa Contoso University pokazuje, jak tworzyć aplikacje ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i programu Visual Studio 2012. Aby uzyskać informacje na temat serii samouczków, zobacz [pierwszy samouczek w serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Możesz uruchomić serię samouczków od początku lub [pobrać początkowy projekt dla tego rozdziału](building-the-ef5-mvc4-chapter-downloads.md) i Zacznij tutaj.
> 
> > [!NOTE] 
> > 
> > Jeśli wystąpi problem, którego nie można rozwiązać, [Pobierz ukończony rozdział](building-the-ef5-mvc4-chapter-downloads.md) i spróbuj odtworzyć problem. Ogólnie rzecz biorąc, można znaleźć rozwiązanie problemu, porównując kod z kompletnym kodem. Niektóre typowe błędy i sposoby ich rozwiązywania można znaleźć w temacie [błędy i obejścia.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)

W poprzednim samouczku użyto dziedziczenia do zredukowania nadmiarowego kodu w `Student` i `Instructor` klas jednostek. W tym samouczku przedstawiono kilka sposobów używania wzorców repozytorium i jednostki pracy dla operacji CRUD. Tak jak w poprzednim samouczku, w tym kroku zmienimy sposób, w jaki kod będzie działać ze stronami, które zostały już utworzone, zamiast tworzyć nowe strony.

## <a name="the-repository-and-unit-of-work-patterns"></a>Wzorce repozytorium i jednostki pracy

Wzorce repozytorium i jednostki pracy są przeznaczone do tworzenia warstwy abstrakcji między warstwą dostępu do danych i warstwą logiki biznesowej aplikacji. Wdrożenie tych wzorców może pomóc w izolowaniu aplikacji od zmian w magazynie danych oraz w celu ułatwienia zautomatyzowanych testów jednostkowych lub projektowania opartego na testach (TDD).

W tym samouczku zaimplementowano klasę repozytorium dla każdego typu jednostki. Dla typu jednostki `Student` utworzysz interfejs repozytorium i klasę repozytorium. Gdy tworzysz wystąpienie repozytorium w kontrolerze, użyjesz interfejsu, aby kontroler zaakceptuje odwołanie do dowolnego obiektu, który implementuje interfejs repozytorium. Gdy kontroler działa na serwerze sieci Web, odbiera repozytorium, które działa z Entity Framework. Gdy kontroler jest uruchamiany w klasie testów jednostkowych, odbiera repozytorium, które działa z danymi przechowywanymi w sposób, który można łatwo manipulować na potrzeby testowania, takich jak kolekcja w pamięci.

W dalszej części tego samouczka użyjesz wielu repozytoriów i klasy jednostki pracy dla `Course` i `Department` typów jednostek na kontrolerze `Course`. Jednostka klasy Work koordynuje prace wielu repozytoriów przez utworzenie pojedynczej klasy kontekstu bazy danych udostępnionej przez wszystkie. Jeśli chcesz mieć możliwość wykonywania zautomatyzowanych testów jednostkowych, Utwórz interfejsy dla tych klas i używaj ich w taki sam sposób, jak w przypadku repozytorium `Student`. Aby jednak zachować ten samouczek, możesz tworzyć i używać tych klas bez interfejsów.

Na poniższej ilustracji przedstawiono jeden ze sposobów konceptualizacji relacji między klasą kontrolera i kontekstu w porównaniu z nieużywanym wzorcem wzorca lub jednostki pracy.

![Repository_pattern_diagram](https://asp.net/media/2578149/Windows-Live-Writer_8c4963ba1fa3_CE3B_Repository_pattern_diagram_1df790d3-bdf2-4c11-9098-946ddd9cd884.png)

Nie utworzysz testów jednostkowych w tej serii samouczków. Aby zapoznać się z wprowadzeniem do programu TDD z aplikacją MVC korzystającą ze wzorca repozytorium, zobacz [Przewodnik: korzystanie z usługi TDD z ASP.NET MVC](https://msdn.microsoft.com/library/ff847525.aspx). Aby uzyskać więcej informacji na temat wzorca repozytorium, zobacz następujące zasoby:

- [Wzorzec repozytorium](https://msdn.microsoft.com/library/ff649690.aspx) w witrynie MSDN.
- [Używanie wzorców repozytorium i jednostki pracy z Entity Framework 4,0](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) na blogu Entity Framework zespołu.
- [Agile Entity Framework 4 repozytorium](http://thedatafarm.com/blog/data-access/agile-entity-framework-4-repository-part-1-model-and-poco-classes/) wpisów w blogu Julie Lerman.
- [Tworzenie konta z błyskawiczną aplikacją HTML5/jQuery](https://weblogs.asp.net/dwahlin/archive/2011/08/15/building-the-account-at-a-glance-html5-jquery-application.aspx) na blogu Dan Wahlin.

> [!NOTE]
> Istnieje wiele sposobów implementacji wzorców repozytorium i jednostki pracy. Można użyć klas repozytorium z klasą jednostki pracy lub bez niej. Można zaimplementować pojedyncze repozytorium dla wszystkich typów jednostek lub jeden dla każdego typu. Jeśli zaimplementowano jeden dla każdego typu, można użyć oddzielnych klas, ogólnej klasy bazowej i klas pochodnych lub abstrakcyjnej klasy bazowej i klas pochodnych. W repozytorium można uwzględnić logikę biznesową lub ograniczyć ją do logiki dostępu do danych. Możesz również utworzyć warstwę abstrakcji w klasie kontekstu bazy danych przy użyciu interfejsów [IDbSet](https://msdn.microsoft.com/library/gg679233(v=vs.103).aspx) zamiast typów [nieogólnymi](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) dla zestawów jednostek. Podejście do implementowania warstwy abstrakcji pokazanej w tym samouczku to jedna z opcji, które należy wziąć pod uwagę, a nie na wszystkie scenariusze i środowiska.

## <a name="creating-the-student-repository-class"></a>Tworzenie klasy repozytorium uczniów

W folderze *dal* Utwórz plik klasy o nazwie *IStudentRepository.cs* i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample1.cs)]

Ten kod deklaruje typowy zestaw metod CRUD, w tym dwie metody odczytu — jeden, który zwraca wszystkie jednostki `Student`, a drugi, który znajduje pojedyncze `Student` jednostki według identyfikatora.

W folderze *dal* Utwórz plik klasy o nazwie *StudentRepository.cs* . Zastąp istniejący kod następującym kodem, który implementuje interfejs `IStudentRepository`:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample2.cs)]

Kontekst bazy danych jest zdefiniowany w zmiennej klasy, a Konstruktor oczekuje, że obiekt wywołujący zostanie przekazany do wystąpienia kontekstu:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample3.cs)]

Można utworzyć wystąpienie nowego kontekstu w repozytorium, ale jeśli w jednym kontrolerze użyto wielu repozytoriów, każda z nich będzie kończyć się niezależnym kontekstem. Później będziesz używać wielu repozytoriów w kontrolerze `Course` i zobaczysz, jak jednostka klasy pracy może zapewnić, że wszystkie repozytoria używają tego samego kontekstu.

Repozytorium implementuje interfejs [IDisposable](https://msdn.microsoft.com/library/system.idisposable.aspx) i usuwa kontekst bazy danych zgodnie z wcześniejszym opisem w kontrolerze, a jego metody CRUD umożliwiają wywoływanie kontekstu bazy danych w taki sam sposób, jak wcześniej.

## <a name="change-the-student-controller-to-use-the-repository"></a>Zmień kontroler ucznia, aby korzystał z repozytorium

W *StudentController.cs*Zastąp kod aktualnie w klasie następującym kodem. Zmiany są wyróżnione.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=13-18,44,75,77,102-103,120,137-138,159,172-174,186)]

Kontroler deklaruje teraz zmienną klasy dla obiektu, który implementuje interfejs `IStudentRepository` zamiast klasy kontekstu:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample5.cs)]

Domyślny Konstruktor (bez parametrów) tworzy nowe wystąpienie kontekstu, a opcjonalny Konstruktor zezwala obiektowi wywołującemu na przekazywanie w wystąpieniu kontekstu.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample6.cs)]

(Jeśli korzystasz z *iniekcji zależności*lub di, nie potrzebujesz domyślnego konstruktora, ponieważ oprogramowanie to zapewnia, że zawsze będzie zapewniony poprawny obiekt repozytorium).

W metodach CRUD repozytorium jest teraz wywoływane zamiast kontekstu:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample7.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample8.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample9.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample10.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample11.cs)]

A metoda `Dispose` teraz usuwa repozytorium zamiast kontekstu:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample12.cs)]

Uruchom witrynę i kliknij kartę **uczniowie** .

![Students_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image1.png)

Strona wygląda i działa tak samo, jak przed zmianą kodu w celu użycia repozytorium, a inne strony ucznia również działają tak samo. Istnieje jednak ważna różnica w sposobie filtrowania i porządkowania metody `Index` kontrolera. Oryginalna wersja tej metody zawierała następujący kod:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample13.cs?highlight=1)]

Zaktualizowana Metoda `Index` zawiera następujący kod:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=1)]

Tylko wyróżniony kod został zmieniony.

W pierwotnej wersji kodu `students` jest wpisana jako obiekt `IQueryable`. Zapytanie nie jest wysyłane do bazy danych, dopóki nie zostanie ono przekonwertowane do kolekcji przy użyciu metody takiej jak `ToList`, która nie występuje do momentu, gdy widok indeksu uzyskuje dostęp do modelu ucznia. Metoda `Where` w oryginalnym kodzie zostanie `WHERE` klauzulą kwerendy SQL, która jest wysyłana do bazy danych. To z kolei oznacza, że baza danych zwraca tylko wybrane jednostki. Jednak w wyniku zmiany `context.Students` na `studentRepository.GetStudents()`, zmienna `students` po tej instrukcji jest kolekcją `IEnumerable`, która obejmuje wszystkich uczniów w bazie danych. Rezultat zastosowania metody `Where` jest taki sam, ale teraz prace odbywają się w pamięci na serwerze sieci Web, a nie w bazie danych programu. W przypadku zapytań, które zwracają duże ilości danych, może to być niewydajne.

> [!TIP]
> 
> **IQueryable a IEnumerable**
> 
> Po zaimplementowaniu repozytorium, jak pokazano tutaj, nawet jeśli wprowadzisz coś w polu **wyszukiwania** , zapytanie wysyłane do SQL Server zwraca wszystkie wiersze ucznia, ponieważ nie zawiera on kryteriów wyszukiwania:
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image2.png)
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample15.sql)]
> 
> To zapytanie zwraca wszystkie dane ucznia, ponieważ repozytorium wykonała zapytanie bez znajomości kryteriów wyszukiwania. Proces sortowania, stosowania kryteriów wyszukiwania i wybierania podzestawu danych na potrzeby stronicowania (wyświetlanie tylko 3 wierszy w tym przypadku) odbywa się później, gdy metoda `ToPagedList` jest wywoływana w kolekcji `IEnumerable`.
> 
> W poprzedniej wersji kodu (przed zaimplementowaniem repozytorium) zapytanie nie jest wysyłane do bazy danych do momentu zastosowania kryteriów wyszukiwania, gdy `ToPagedList` jest wywoływana dla `IQueryable` obiektu.
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image3.png)
> 
> Gdy ToPagedList jest wywoływana dla obiektu `IQueryable`, zapytanie wysyłane do SQL Server określa ciąg wyszukiwania, a jako wynik są zwracane tylko wiersze, które spełniają kryteria wyszukiwania, i nie trzeba wykonywać filtrowania w pamięci.
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample16.sql)]
> 
> (W poniższym samouczku wyjaśniono, jak sprawdzać zapytania wysyłane do SQL Server).

W poniższej sekcji pokazano, jak zaimplementować metody repozytorium, które umożliwiają określenie, że ta czynność powinna zostać wykonana przez bazę danych.

Warstwa abstrakcji została teraz utworzona między kontrolerem a kontekstem bazy danych Entity Framework. Jeśli chcesz przeprowadzić zautomatyzowane testowanie jednostkowe za pomocą tej aplikacji, możesz utworzyć alternatywną klasę repozytorium w projekcie testów jednostkowych, który implementuje `IStudentRepository` *.* Zamiast wywołania kontekstu do odczytu i zapisu danych, ta klasa repozytorium może manipulować kolekcjami w pamięci w celu przetestowania funkcji kontrolera.

## <a name="implement-a-generic-repository-and-a-unit-of-work-class"></a>Implementowanie repozytorium ogólnego i klasy jednostki pracy

Utworzenie klasy repozytorium dla każdego typu jednostki może spowodować powstanie wielu nadmiarowego kodu i może spowodować częściowe aktualizacje. Załóżmy na przykład, że trzeba zaktualizować dwa różne typy jednostek w ramach tej samej transakcji. Jeśli każda z nich używa oddzielnego wystąpienia kontekstu bazy danych, jeden może się powieść, a druga może się nie powieść. Jednym ze sposobów minimalizowania nadmiarowego kodu jest użycie ogólnego repozytorium, a jednym ze sposobów zapewnienia, że wszystkie repozytoria używają tego samego kontekstu bazy danych (i w związku z tym koordynują wszystkie aktualizacje), ma używać jednostki klasy pracy.

W tej części samouczka utworzysz klasę `GenericRepository` i klasę `UnitOfWork`, a następnie użyjesz ich na kontrolerze `Course`, aby uzyskać dostęp do zestawów jednostek `Department` i `Course`. Jak wyjaśniono wcześniej, aby ta część samouczka była prosta, nie tworzysz interfejsów dla tych klas. Ale jeśli zamierzasz korzystać z nich w celu ułatwienia korzystania z nich, zazwyczaj Wdrażaj je przy użyciu interfejsów w taki sam sposób, jak repozytorium `Student`.

### <a name="create-a-generic-repository"></a>Tworzenie repozytorium ogólnego

W folderze *dal* Utwórz *GenericRepository.cs* i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample17.cs)]

Zmienne klasy są deklarowane dla kontekstu bazy danych i dla zestawu jednostek, dla którego jest tworzone wystąpienie repozytorium:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample18.cs)]

Konstruktor akceptuje wystąpienie kontekstu bazy danych i inicjuje zmienną zestawu jednostek:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample19.cs)]

Metoda `Get` używa wyrażeń lambda, aby umożliwić kod wywołujący, aby określić warunek filtru i kolumnę, w której mają być uporządkowane wyniki, a parametr String umożliwia obiektowi wywołującemu przeznaczenie listy właściwości nawigacji do eager ładowania:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample20.cs)]

Kod `Expression<Func<TEntity, bool>> filter` oznacza, że obiekt wywołujący dostarczy wyrażenie lambda na podstawie typu `TEntity`, a to wyrażenie zwróci wartość logiczną. Na przykład jeśli repozytorium jest tworzone dla typu jednostki `Student`, kod w metodzie wywołującej może określić `student => student.LastName == "Smith`&quot; dla parametru `filter`.

Kod `Func<IQueryable<TEntity>, IOrderedQueryable<TEntity>> orderBy` oznacza również, że obiekt wywołujący dostarczy wyrażenie lambda. Ale w tym przypadku dane wejściowe do wyrażenia są obiektem `IQueryable` dla typu `TEntity`. Wyrażenie zwróci uporządkowaną wersję tego obiektu `IQueryable`. Na przykład jeśli repozytorium jest tworzone dla typu jednostki `Student`, kod w metodzie wywołującej może określić `q => q.OrderBy(s => s.LastName)` parametru `orderBy`.

Kod w metodzie `Get` tworzy obiekt `IQueryable`, a następnie stosuje wyrażenie filtru, jeśli istnieje:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample21.cs)]

Następnie stosuje wyrażenia ładowania eager po przeanalizowaniu listy rozdzielanej przecinkami:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample22.cs)]

Na koniec stosuje wyrażenie `orderBy`, jeśli istnieje i zwraca wyniki. w przeciwnym razie zwraca wyniki z kwerendy nieuporządkowanej:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample23.cs)]

Po wywołaniu metody `Get` można filtrować i sortować według `IEnumerable`j kolekcji zwróconej przez metodę zamiast dostarczać parametry dla tych funkcji. Jednak zadania sortowania i filtrowania są następnie wykonywane w pamięci na serwerze sieci Web. Korzystając z tych parametrów, należy się upewnić, że prace są wykonywane przez bazę danych, a nie na serwerze sieci Web. Alternatywą jest tworzenie klas pochodnych dla określonych typów jednostek i Dodawanie wyspecjalizowanych metod `Get`, takich jak `GetStudentsInNameOrder` lub `GetStudentsByName`. Jednak w złożonej aplikacji może to spowodować powstanie dużej liczby takich klas pochodnych i wyspecjalizowanych metod, co może obsłużyć więcej pracy.

Kod w metodach `GetByID`, `Insert`i `Update` jest podobny do tego, co zostało nadane w repozytorium nieogólnym. (W sygnaturze `GetByID` nie jest podawany parametr ładowania eager, ponieważ nie można wykonać ładowania eager przy użyciu metody `Find`).

Dla metody `Delete` są dostępne dwa przeciążenia:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample24.cs)]

Jeden z tych umożliwia przekazanie tylko identyfikatora jednostki do usunięcia, a jednym z nich jest wystąpienie jednostki. Jak przedstawiono w samouczku [Obsługa współbieżności](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) , na potrzeby obsługi współbieżności potrzebujesz metody `Delete`, która przyjmuje wystąpienie jednostki, które zawiera oryginalną wartość właściwości śledzenia.

To ogólne repozytorium będzie obsługiwało typowe wymagania CRUD. Jeśli określony typ jednostki ma specjalne wymagania, takie jak bardziej złożone filtrowanie lub porządkowanie, można utworzyć klasę pochodną, która ma dodatkowe metody dla tego typu.

## <a name="creating-the-unit-of-work-class"></a>Tworzenie jednostki klasy pracy

Jednostka klasy pracy służy do jednego celu: aby upewnić się, że w przypadku korzystania z wielu repozytoriów, współużytkują one kontekst pojedynczej bazy danych. W ten sposób, gdy jednostka pracy jest ukończona, można wywołać metodę `SaveChanges` w tym wystąpieniu kontekstu i mieć pewność, że wszystkie powiązane zmiany zostaną skoordynowane. Wszystko, co potrzebuje Klasa, jest metodą `Save` i właściwością dla każdego repozytorium. Każda właściwość repozytorium zwraca wystąpienie repozytorium, które zostało utworzone przy użyciu tego samego wystąpienia kontekstu bazy danych co inne wystąpienia repozytorium.

W folderze *dal* Utwórz plik klasy o nazwie *UnitOfWork.cs* i Zastąp kod szablonu następującym kodem:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample25.cs)]

Kod tworzy zmienne klas dla kontekstu bazy danych i każdego repozytorium. Dla zmiennej `context` jest tworzone wystąpienie nowego kontekstu:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample26.cs)]

Każda właściwość repozytorium sprawdza, czy repozytorium już istnieje. W przeciwnym razie tworzy wystąpienie repozytorium, przekazując w wystąpienie kontekstu. W związku z tym wszystkie repozytoria współużytkują to samo wystąpienie kontekstu.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample27.cs)]

Metoda `Save` wywołuje `SaveChanges` w kontekście bazy danych.

Podobnie jak w przypadku każdej klasy, która tworzy wystąpienie kontekstu bazy danych w zmiennej klasy, Klasa `UnitOfWork` implementuje `IDisposable` i usuwa kontekst.

### <a name="changing-the-course-controller-to-use-the-unitofwork-class-and-repositories"></a>Zmiana kontrolera kursu w celu używania klasy UnitOfWork i repozytoriów

Zastąp kod aktualnie w *CourseController.cs* następującym kodem:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample28.cs?highlight=15,20,22,31,54-55,70,85-86,101-102,122-124,130)]

Ten kod dodaje zmienną klasy dla klasy `UnitOfWork`. (Jeśli w tym miejscu używasz interfejsów, nie można zainicjować zmiennej tutaj. zamiast tego należy zaimplementować wzorzec dwóch konstruktorów tak samo jak w przypadku repozytorium `Student`.)

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample29.cs)]

W pozostałej części klasy wszystkie odwołania do kontekstu bazy danych są zastępowane odwołaniami do odpowiedniego repozytorium, przy użyciu `UnitOfWork` właściwości, aby uzyskać dostęp do repozytorium. Metoda `Dispose` usuwa wystąpienie `UnitOfWork`.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample30.cs)]

Uruchom witrynę i kliknij kartę **kursy** .

![Courses_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image4.png)

Strona wygląda i działa tak samo jak przed zmianami, a inne strony kursu również działają tak samo.

## <a name="summary"></a>Podsumowanie

Wdrożono zarówno wzorce repozytorium, jak i jednostki pracy. Użyto wyrażeń lambda jako parametrów metody w repozytorium ogólnym. Aby uzyskać więcej informacji na temat używania tych wyrażeń z obiektem `IQueryable`, zobacz [interfejs IQueryable (t) Interface (System. LINQ)](https://msdn.microsoft.com/library/bb351562.aspx) w bibliotece MSDN. W następnym samouczku dowiesz się, jak obsługiwać niektóre zaawansowane scenariusze.

Linki do innych zasobów Entity Framework można znaleźć w [mapie zawartości dostęp do danych ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Poprzednie](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [dalej](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)

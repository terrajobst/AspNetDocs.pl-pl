---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: Sortowanie, filtrowanie i stronicowanie za pomocą Entity Framework w aplikacji ASP.NET MVC (3 z 10) | Microsoft Docs
author: tdykstra
description: Przykładowa aplikacja internetowa Contoso University pokazuje, jak tworzyć aplikacje ASP.NET MVC 4 przy użyciu Code First Entity Framework 5 i programu Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 8af630e0-fffa-4110-9eca-c96e201b2724
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: b1ddb70805dcb07fb60eea895ff572c054bde5c6
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74595221"
---
# <a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application-3-of-10"></a>Sortowanie, filtrowanie i stronicowanie przy użyciu Entity Framework w aplikacji ASP.NET MVC (3 z 10)

Autor [Dykstra](https://github.com/tdykstra)

[Pobierz ukończony projekt](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Przykładowa aplikacja internetowa Contoso University pokazuje, jak tworzyć aplikacje ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i programu Visual Studio 2012. Aby uzyskać informacje na temat serii samouczków, zobacz [pierwszy samouczek w serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Możesz uruchomić serię samouczków od początku lub [pobrać początkowy projekt dla tego rozdziału](building-the-ef5-mvc4-chapter-downloads.md) i Zacznij tutaj.
> 
> > [!NOTE] 
> > 
> > Jeśli wystąpi problem, którego nie można rozwiązać, [Pobierz ukończony rozdział](building-the-ef5-mvc4-chapter-downloads.md) i spróbuj odtworzyć problem. Ogólnie rzecz biorąc, można znaleźć rozwiązanie problemu, porównując kod z kompletnym kodem. Niektóre typowe błędy i sposoby ich rozwiązywania można znaleźć w temacie [błędy i obejścia.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)

W poprzednim samouczku zaimplementowano zestaw stron sieci Web dla podstawowych operacji CRUD dla jednostek `Student`. W tym samouczku dodasz funkcje sortowania, filtrowania i stronicowania na stronie indeksu **uczniów** . Utworzysz również stronę, która będzie prostą grupowaniem.

Na poniższej ilustracji przedstawiono, jak będzie wyglądać strona po zakończeniu. Nagłówki kolumn to linki, które użytkownik może kliknąć, aby posortować według tej kolumny. Wielokrotne kliknięcie nagłówka kolumny powoduje przełączenie między rosnącą i malejącą kolejnością sortowania.

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>Dodaj linki do sortowania kolumn na stronie indeksu uczniów

Aby dodać sortowanie na stronie indeksu ucznia, należy zmienić metodę `Index` kontrolera `Student` i dodać kod do widoku indeksu `Student`.

### <a name="add-sorting-functionality-to-the-index-method"></a>Dodawanie funkcji sortowania do metody index

W *Controllers\StudentController.cs*Zastąp metodę `Index` następującym kodem:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Ten kod otrzymuje parametr `sortOrder` z ciągu zapytania w adresie URL. Wartość ciągu zapytania jest dostarczana przez ASP.NET MVC jako parametr do metody akcji. Parametr będzie ciągiem "name" lub "date", opcjonalnie po znaku podkreślenia i ciągu "DESC" w celu określenia kolejności malejącej. Domyślna kolejność sortowania to Ascending.

Podczas pierwszego żądania strony indeksu nie ma ciągu zapytania. Studenci są wyświetlani w kolejności rosnącej według `LastName`, która jest wartością domyślną ustanowioną przez przypadek przypadania w instrukcji `switch`. Gdy użytkownik kliknie hiperlink nagłówka kolumny, odpowiednia wartość `sortOrder` jest podawana w ciągu zapytania.

Dwie zmienne `ViewBag` są używane, aby widok mógł skonfigurować hiperłącza nagłówków kolumn z odpowiednimi wartościami ciągu zapytania:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Są to "Trzyelementowy". Pierwszy z nich określa, że jeśli parametr `sortOrder` ma wartość null lub jest pusty, `ViewBag.NameSortParm` powinna być ustawiona na wartość "name\_DESC"; w przeciwnym razie należy ustawić pusty ciąg. Te dwie instrukcje umożliwiają widokowi ustawienie hiperłączy nagłówka kolumny w następujący sposób:

| Bieżący porządek sortowania | Hiperłącze nazwisko | Hiperłącze daty |
| --- | --- | --- |
| Nazwisko — rosnąco | descending | ascending |
| Nazwisko malejąco | ascending | ascending |
| Data rosnąca | ascending | descending |
| Data malejąca | ascending | ascending |

Metoda używa [LINQ to Entities](https://msdn.microsoft.com/library/bb386964.aspx) , aby określić kolumnę, według której ma zostać wykonane sortowanie. Kod tworzy zmienną [IQueryable](https://msdn.microsoft.com/library/bb351562.aspx) przed instrukcją `switch`, modyfikuje ją w instrukcji `switch` i wywołuje metodę `ToList` po instrukcji `switch`. Gdy tworzysz i modyfikujesz zmienne `IQueryable`, żadne zapytanie nie jest wysyłane do bazy danych. Zapytanie nie jest wykonywane do momentu przekonwertowania obiektu `IQueryable` do kolekcji przez wywołanie metody, takiej jak `ToList`. W związku z tym ten kod skutkuje pojedynczym zapytaniem, które nie jest wykonywane do momentu instrukcji `return View`.

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Dodawanie hiperłączy nagłówka kolumny do widoku indeksu ucznia

W *Views\Student\Index.cshtml*zastąp `<tr>` i `<th>` elementów dla wiersza nagłówka z wyróżnionym kodem:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

Ten kod używa informacji we właściwościach `ViewBag`, aby skonfigurować hiperłącza z odpowiednimi wartościami ciągu zapytania.

Uruchom stronę i kliknij nagłówki kolumny **nazwisko** i **Data rejestracji** , aby sprawdzić, czy sortowanie działa.

![Students_Index_page_with_sort_hyperlinks](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Po **kliknięciu nagłówka nazwisko** uczniowie będą wyświetlane w kolejności malejących nazwisk.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>Dodawanie pola wyszukiwania do strony indeksu uczniów

Aby dodać filtrowanie do strony indeksu uczniów, należy dodać pole tekstowe i przycisk Prześlij do widoku i wprowadzić odpowiednie zmiany w metodzie `Index`. Pole tekstowe umożliwia wprowadzenie ciągu do wyszukania w polach Imię i nazwisko.

### <a name="add-filtering-functionality-to-the-index-method"></a>Dodawanie funkcji filtrowania do metody index

W *Controllers\StudentController.cs*Zastąp metodę `Index` następującym kodem (zmiany są wyróżnione):

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

Do metody `Index` został dodany parametr `searchString`. Dodano również do instrukcji LINQ `where` klauzulę, która wybiera tylko uczniów, których imię lub nazwisko zawiera ciąg wyszukiwania. Wartość ciągu wyszukiwania jest odbierana z pola tekstowego, które zostanie dodane do widoku indeksu. Instrukcja, która dodaje klauzulę [WHERE](https://msdn.microsoft.com/library/bb535040.aspx) , jest wykonywana tylko wtedy, gdy istnieje wartość do wyszukania.

> [!NOTE]
> W wielu przypadkach można wywołać tę samą metodę w ramach zestawu jednostek Entity Framework lub jako metodę rozszerzenia w kolekcji w pamięci. Wyniki są zwykle takie same, ale w niektórych przypadkach mogą być różne. Na przykład .NET Framework implementacja metody `Contains` zwraca wszystkie wiersze w przypadku przekazania do niego pustego ciągu, ale dostawca Entity Framework dla SQL Server Compact 4,0 zwraca zero wierszy dla pustych ciągów. W związku z tym kod w przykładzie (umieszczenie instrukcji `Where` wewnątrz instrukcji `if`) gwarantuje, że te same wyniki są uzyskiwane dla wszystkich wersji SQL Server. Ponadto .NET Framework implementacja metody `Contains` domyślnie wykonuje porównanie z uwzględnieniem wielkości liter, ale Entity Framework dostawca SQL Server domyślnie wykonuje porównania bez uwzględniania wielkości liter. W związku z tym, wywołując metodę `ToUpper`, aby test jawnie nie uwzględniał wielkości liter, nie zmienia się w przypadku późniejszej zmiany kodu w celu użycia repozytorium, które zwróci `IEnumerable` kolekcji zamiast obiektu `IQueryable`. (W przypadku wywołania metody `Contains` w kolekcji `IEnumerable` zostanie .NET Framework implementacja. po wywołaniu dla obiektu `IQueryable` zostanie wykorzystana implementacja dostawcy bazy danych).

### <a name="add-a-search-box-to-the-student-index-view"></a>Dodawanie pola wyszukiwania do widoku indeksu ucznia

W *Views\Student\Index.cshtml*, Dodaj wyróżniony kod bezpośrednio przed otwierającym tagiem `table`, aby utworzyć podpis, pole tekstowe i przycisk **wyszukiwania** .

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

Uruchom stronę, wprowadź ciąg wyszukiwania, a następnie kliknij przycisk **Wyszukaj** , aby sprawdzić, czy filtrowanie działa.

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Zwróć uwagę, że adres URL nie zawiera ciągu wyszukiwania "a", co oznacza, że jeśli utworzysz zakładkę na tej stronie, nie otrzymasz filtrowanej listy przy użyciu zakładki. Zmienisz przycisk **wyszukiwania** , aby używać ciągów zapytania dla kryteriów filtrowania w dalszej części tego samouczka.

## <a name="add-paging-to-the-students-index-page"></a>Dodawanie stronicowania do strony indeksu uczniów

Aby dodać stronicowanie do strony indeksu uczniów, Zacznij od zainstalowania pakietu NuGet **PagedList. MVC** . Następnie wprowadzisz dodatkowe zmiany w metodzie `Index` i dodasz linki stronicowania do widoku `Index`. **PagedList. MVC** to jeden z wielu dobrych pakietów stronicowania i sortowania dla ASP.NET MVC, a jego użycie w tym miejscu jest przeznaczone tylko jako przykład, a nie jako zalecenia dotyczące innych opcji. Na poniższej ilustracji przedstawiono linki stronicowania.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>Zainstaluj pakiet NuGet PagedList. MVC

Pakiet NuGet **PagedList. MVC** automatycznie instaluje pakiet **PagedList** jako zależność. Pakiet **PagedList** instaluje `PagedList` typ kolekcji i metody rozszerzające dla kolekcji `IQueryable` i `IEnumerable`. Metody rozszerzające umożliwiają utworzenie pojedynczej strony danych w kolekcji `PagedList` z `IQueryable` lub `IEnumerable`, a kolekcja `PagedList` zawiera kilka właściwości i metod, które ułatwiają stronicowanie. Pakiet **PagedList. MVC** instaluje pomocnika stronicowania, który wyświetla przyciski stronicowania.

W menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet** , a następnie **Zarządzaj pakietami NuGet dla rozwiązania**.

W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij kartę **online** po lewej stronie, a następnie wprowadź "stronicowane" w polu wyszukiwania. Gdy zobaczysz pakiet **PagedList. MVC** , kliknij przycisk **Instaluj**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

W polu **Wybierz projekty** kliknij przycisk **OK**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="add-paging-functionality-to-the-index-method"></a>Dodawanie funkcji stronicowania do metody index

W *Controllers\StudentController.cs*dodaj instrukcję `using` dla przestrzeni nazw `PagedList`:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Zastąp metodę `Index` następującym kodem:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Ten kod dodaje parametr `page`, bieżący parametr kolejności sortowania i bieżący parametr filtru do sygnatury metody, jak pokazano poniżej:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Gdy strona jest wyświetlana po raz pierwszy lub jeśli użytkownik nie kliknął linku stronicowania ani sortowania, wszystkie parametry będą mieć wartość null. Jeśli zostanie kliknięty link stronicowania, zmienna `page` będzie zawierać numer strony do wyświetlenia.

Właściwość `A ViewBag` zapewnia widok z bieżącą kolejnością sortowania, ponieważ musi być uwzględniony w łączach stronicowania, aby zachować porządek sortowania w tym samym czasie podczas stronicowania:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Inna właściwość, `ViewBag.CurrentFilter`, udostępnia widok z bieżącym ciągiem filtru. Ta wartość musi być uwzględniona w łączach stronicowania, aby zachować ustawienia filtru podczas stronicowania, i musi zostać przywrócone do pola tekstowego, gdy strona jest ponownie wyświetlana. Jeśli ciąg wyszukiwania zostanie zmieniony podczas stronicowania, należy zresetować stronę do 1, ponieważ nowy filtr może spowodować wyświetlenie różnych danych. Ciąg wyszukiwania jest zmieniany, gdy wartość zostanie wprowadzona w polu tekstowym, a przycisk Prześlij zostanie naciśnięty. W takim przypadku parametr `searchString` nie ma wartości null.

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Na końcu metody `ToPagedList` Metoda rozszerzenia w obiekcie Students `IQueryable` konwertuje zapytanie ucznia na jedną stronę uczniów w typie kolekcji, który obsługuje stronicowanie. Ta pojedyncza strona studentów jest następnie przenoszona do widoku:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Metoda `ToPagedList` przyjmuje numer strony. Dwa znaki zapytania reprezentują [operator łączenia wartości null](https://msdn.microsoft.com/library/ms173224.aspx). Operator łączenia wartości null definiuje wartość domyślną dla typu dopuszczającego wartość null; wyrażenie `(page ?? 1)` oznacza zwrócenie wartości `page`, jeśli ma wartość, lub zwraca wartość 1, jeśli `page` jest wartością null.

### <a name="add-paging-links-to-the-student-index-view"></a>Dodawanie linków stronicowania do widoku indeksu ucznia

W *Views\Student\Index.cshtml*Zastąp istniejący kod następującym kodem:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=6,9,14-20,56-58)]

Instrukcja `@model` w górnej części strony określa, że widok pobiera teraz obiekt `PagedList` zamiast obiektu `List`.

Instrukcja `using` dla `PagedList.Mvc` daje dostęp do pomocnika MVC dla przycisków stronicowania.

Kod używa przeciążenia elementu [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) , który umożliwia określenie [FormMethod. Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css).

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

Wartość domyślna [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) przesyła dane formularza z wpisem, co oznacza, że parametry są przesyłane w treści wiadomości HTTP, a nie w adresie URL jako ciągi zapytań. Po określeniu protokołu HTTP GET dane formularza są przesyłane w adresie URL jako ciągi zapytań, co umożliwia użytkownikom tworzenie zakładek w adresie URL. [Wskazówki W3C dotyczące korzystania z protokołu HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) określają, że należy używać Get, gdy akcja nie spowoduje aktualizacji.

Pole tekstowe jest inicjowane z bieżącym ciągiem wyszukiwania, więc po kliknięciu nowej strony można zobaczyć bieżący ciąg wyszukiwania.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

Linki nagłówka kolumny używają ciągu zapytania, aby przekazać bieżący ciąg wyszukiwania do kontrolera, tak aby użytkownik mógł sortować wyniki filtrów:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

Zostanie wyświetlona bieżąca strona i całkowita liczba stron.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

Jeśli nie ma żadnych stron do wyświetlenia, wyświetlana jest wartość "Strona 0 z 0". (W tym przypadku numer strony jest większy niż liczba stron, ponieważ `Model.PageNumber` wynosi 1, a `Model.PageCount` to 0).

Przyciski stronicowania są wyświetlane przez pomocnika `PagedListPager`:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

Pomocnik `PagedListPager` udostępnia wiele opcji, które można dostosować, w tym adresy URL i style. Aby uzyskać więcej informacji, zobacz [TroyGoode/PagedList](https://github.com/TroyGoode/PagedList) w witrynie GitHub.

Uruchom stronę.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Kliknij linki stronicowania w różnych kolejności sortowania, aby upewnić się, że stronicowanie działa. Następnie wprowadź ciąg wyszukiwania i spróbuj ponownie wykonać stronicowanie, aby sprawdzić, czy stronicowanie działa prawidłowo z sortowaniem i filtrowaniem.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>Utwórz stronę informacje z informacjami o statystyce uczniów

Na stronie informacje o witrynie sieci Web dla uniwersytetów firmy Contoso można wyświetlić liczbę studentów zarejestrowanych dla każdej daty rejestracji. Wymaga to grupowania i prostych obliczeń dla grup. Aby to osiągnąć, należy wykonać następujące czynności:

- Utwórz klasę modelu widoku dla danych, które należy przekazać do widoku.
- Zmodyfikuj metodę `About` na kontrolerze `Home`.
- Zmodyfikuj widok `About`.

### <a name="create-the-view-model"></a>Tworzenie modelu widoku

Utwórz folder *modele widoków* . W tym folderze Dodaj plik klasy *EnrollmentDateGroup.cs* i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>Modyfikowanie kontrolera macierzystego

W *HomeController.cs*Dodaj następujące instrukcje `using` w górnej części pliku:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Dodaj zmienną klasy dla kontekstu bazy danych bezpośrednio po otwierającym nawiasie klamrowym dla klasy:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

Zastąp metodę `About` następującym kodem:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

Instrukcja LINQ grupuje jednostki uczniów według daty rejestracji, oblicza liczbę jednostek w każdej grupie i zapisuje wyniki w kolekcji `EnrollmentDateGroup` widoku obiektów modelu.

Dodaj metodę `Dispose`:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>Modyfikowanie widoku informacje

Zastąp kod w pliku *Views\Home\About.cshtml* następującym kodem:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

Uruchom aplikację i kliknij łącze **informacje** . W tabeli zostanie wyświetlona liczba uczniów dla każdej daty rejestracji.

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

## <a name="optional-deploy-the-app-to-windows-azure"></a>Opcjonalne: wdrażanie aplikacji w systemie Windows Azure

Do tej pory aplikacja została uruchomiona lokalnie w IIS Express na komputerze deweloperskim. Aby udostępnić je innym osobom, które będą mogły korzystać z Internetu, należy wdrożyć je dla dostawcy hostingu w sieci Web. W tej opcjonalnej sekcji samouczka wdrożono ją w witrynie sieci Web systemu Windows Azure.

### <a name="using-code-first-migrations-to-deploy-the-database"></a>Wdrażanie bazy danych przy użyciu Migracje Code First

Do wdrożenia bazy danych, która będzie używana Migracje Code First. Podczas tworzenia profilu publikowania, który służy do konfigurowania ustawień wdrożenia z programu Visual Studio, należy zaznaczyć pole wyboru z etykietą **wykonaj migracje Code First (uruchamia się przy uruchamianiu aplikacji)** . To ustawienie powoduje, że proces wdrażania automatycznie skonfiguruje plik *Web. config* aplikacji na serwerze docelowym, tak aby Code First używa klasy inicjatora `MigrateDatabaseToLatestVersion`.

Program Visual Studio nie robi niczego w przypadku bazy danych podczas procesu wdrażania. Gdy wdrożona aplikacja uzyskuje dostęp do bazy danych po raz pierwszy po wdrożeniu, Code First automatycznie tworzy bazę danych lub aktualizuje schemat bazy danych do najnowszej wersji. Jeśli aplikacja implementuje metodę `Seed` migracji, Metoda zostanie uruchomiona po utworzeniu bazy danych lub zaktualizowaniu schematu.

Migracja `Seed` Metoda wstawia dane testowe. W przypadku wdrażania w środowisku produkcyjnym należy zmienić metodę `Seed` tak, aby wstawiali tylko dane, które mają zostać wstawione do produkcyjnej bazy danych. Na przykład w bieżącym modelu danych może być konieczne posiadanie rzeczywistych kursów, ale fikcyjnych uczniów w bazie danych programistycznych. Można napisać metodę `Seed`, aby załadować zarówno w fazie tworzenia, jak i dodać komentarz do fikcyjnych uczniów przed wdrożeniem w środowisku produkcyjnym. Możesz też napisać `Seed` metodę, aby załadować tylko kursy i ręcznie wprowadzić fikcyjne uczniów w bazie danych testowych przy użyciu interfejsu użytkownika aplikacji.

### <a name="get-a-windows-azure-account"></a>Pobierz konto platformy Microsoft Azure

Będziesz potrzebować konta platformy Microsoft Azure. Jeśli jeszcze tego nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz [bezpłatna wersja próbna platformy Microsoft Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

### <a name="create-a-web-site-and-a-sql-database-in-windows-azure"></a>Tworzenie witryny sieci Web i bazy danych SQL w systemie Windows Azure

Witryna sieci Web systemu Windows Azure będzie działać w udostępnionym środowisku hostingu, co oznacza, że jest ono uruchamiane na maszynach wirtualnych, które są udostępniane innym klientom systemu Windows Azure. Udostępnione środowisko hostingu jest tanim sposobem na rozpoczęcie pracy w chmurze. Później, jeśli ruch internetowy zostanie zwiększony, aplikacja może skalować się, aby zaspokoić potrzeby, uruchamiając na dedykowanych maszynach wirtualnych. Jeśli potrzebujesz bardziej złożonej architektury, możesz przeprowadzić migrację do usługi w chmurze platformy Microsoft Azure. Usługi w chmurze są uruchamiane na dedykowanych maszynach wirtualnych, które można skonfigurować zgodnie z potrzebami.

Windows Azure SQL Database jest usługą relacyjnej bazy danych opartą na chmurze, która jest oparta na technologiach SQL Server. Narzędzia i aplikacje, które działają z SQL Server również współpracują z SQL Database.

1. Na [platformie Microsoft Azure Portal zarządzania](https://manage.windowsazure.com/)kliknij pozycję **witryny sieci Web** na lewej karcie, a następnie kliknij pozycję **Nowy**.

    ![Przycisk Nowy w portal zarządzania](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)
2. Kliknij pozycję **Tworzenie niestandardowe**.

    ![Utwórz za pomocą linku bazy danych w portal zarządzania](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

   Zostanie otwarta **Nowa witryna sieci Web — Kreator tworzenia niestandardowego** .
3. W kroku kreatora **nowej witryny sieci Web** wprowadź ciąg w polu **adresu URL** , który będzie używany jako unikatowy adres URL aplikacji. Pełny adres URL będzie zawierać dane wprowadzane tutaj oraz sufiks wyświetlany obok pola tekstowego. Na ilustracji przedstawiono wartość "ConU", ale ten adres URL jest prawdopodobnie potrzebny do wybrania innego.

    ![Utwórz za pomocą linku bazy danych w portal zarządzania](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)
4. Z listy rozwijanej **region** wybierz region blisko siebie. To ustawienie określa centrum danych, w którym zostanie uruchomiona witryna sieci Web.
5. Z listy rozwijanej **baza danych** wybierz pozycję **Utwórz bezpłatną bazę danych SQL o rozmiarze 20 MB**.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)
6. W polu **nazwa parametrów połączenia bazy danych**wprowadź *SchoolContext*.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)
7. Kliknij strzałkę wskazującą w prawo u dołu pola. Kreator przechodzi do kroku **Ustawienia bazy danych** .
8. W polu **Nazwa** wprowadź *ContosoUniversityDB*.
9. W polu **serwer** wybierz pozycję **nowy serwer SQL Database**. Alternatywnie, jeśli wcześniej utworzono serwer, można wybrać ten serwer z listy rozwijanej.
10. Wprowadź **nazwę logowania** i **hasło**administratora. Jeśli wybrano opcję **nowy serwer SQL Database** nie wprowadzono w tym miejscu istniejącej nazwy i hasła, zostanie wprowadzona nowa nazwa i hasło, które będą używane później podczas uzyskiwania dostępu do bazy danych. W przypadku wybrania wcześniej utworzonego serwera należy wprowadzić poświadczenia dla tego serwera. W tym samouczku nie zostanie zaznaczone pole wyboru ***Zaawansowane*** . Opcje ***Zaawansowane*** umożliwiają ustawienie [sortowania](https://msdn.microsoft.com/library/aa174903(v=SQL.80).aspx)bazy danych.
11. Wybierz ten sam **region** , który został wybrany dla witryny sieci Web.
12. Kliknij znacznik wyboru w prawym dolnym rogu pola, aby wskazać, że wszystko zostało zakończone.   
  
    ![Krok ustawień bazy danych nowej witryny sieci Web — Kreator tworzenia z bazą danych](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image16.png)  

    Na poniższej ilustracji przedstawiono korzystanie z istniejących SQL Server i logowania.   
  
    ![Krok ustawień bazy danych nowej witryny sieci Web — Kreator tworzenia z bazą danych](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image17.png)  
  
    Portal zarządzania wraca do strony witryny sieci Web, a kolumna **stan** wskazuje, że witryna jest tworzona. Po czasie (zazwyczaj krótszym niż minutę) kolumna **stan** pokazuje, że witryna została pomyślnie utworzona. Na pasku nawigacyjnym po lewej stronie liczba witryn znajdujących się na koncie pojawia się obok ikony **witryny sieci Web** , a liczba baz danych pojawia się obok ikony **bazy danych SQL** .

## <a name="deploy-the-application-to-windows-azure"></a>Wdrażanie aplikacji w systemie Windows Azure

1. W programie Visual Studio kliknij prawym przyciskiem myszy projekt w **Eksplorator rozwiązań** i wybierz polecenie **Publikuj** z menu kontekstowego.  
  
    ![Menu kontekstowe publikowania w projekcie](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image18.png)
2. Na karcie **profil** Kreatora publikacji w **sieci Web** kliknij pozycję **Importuj**.  
  
    ![Importuj ustawienia publikowania](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image19.png)
3. Jeśli nie dodano wcześniej subskrypcji systemu Windows Azure w programie Visual Studio, wykonaj następujące czynności. W tych krokach dodajesz subskrypcję, aby lista rozwijana **importowana z witryny sieci Web systemu Windows Azure** obejmowała witrynę sieci Web.

    a. W oknie dialogowym **Importowanie profilu publikowania** kliknij pozycję **Importuj z witryny sieci Web systemu Windows Azure**, a następnie kliknij pozycję **Dodaj subskrypcję systemu Windows Azure**.

    ![Dodaj subskrypcję systemu Windows Azure](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image20.png)

    b. W oknie dialogowym **Importowanie subskrypcji systemu Windows Azure** kliknij pozycję **Pobierz plik subskrypcji**.

    ![Pobierz plik subskrypcji](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image21.png)

    s. W oknie przeglądarki Zapisz plik *. publishsettings* .

    ![Pobierz plik. publishsettings](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image22.png)

    > [!WARNING]
    > Zabezpieczenia — plik *publishsettings* zawiera Twoje poświadczenia (niekodowane), które są używane do administrowania subskrypcjami i usługami systemu Windows Azure. Najlepszym rozwiązaniem w zakresie zabezpieczeń dla tego pliku jest przechowywanie go tymczasowo poza katalogami źródłowymi (na przykład w folderze *Libraries\Documents* ), a następnie usunięcie go po zakończeniu importu. Złośliwy użytkownik, który uzyskuje dostęp do pliku `.publishsettings`, może edytować, tworzyć i usuwać usługi systemu Windows Azure.

    Wykres. W oknie dialogowym **Importowanie subskrypcji systemu Windows Azure** kliknij przycisk **Przeglądaj** i przejdź do pliku *publishsettings* .

    ![Pobierz podrzędny](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image23.png)

    adres. Kliknij pozycję **Importuj**.

    ![import](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image24.png)
4. W oknie dialogowym **Importowanie profilu publikowania** wybierz pozycję **Importuj z witryny sieci Web systemu Windows Azure**, wybierz witrynę sieci Web z listy rozwijanej, a następnie kliknij przycisk **OK**.  
  
    ![Importuj profil publikowania](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image25.png)
5. Na karcie **połączenie** kliknij pozycję **Sprawdź poprawność połączenia** , aby upewnić się, że ustawienia są poprawne.  
  
    ![Weryfikuj połączenie](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)
6. Po sprawdzeniu poprawności połączenia zostanie wyświetlony zielony znacznik wyboru obok przycisku **Weryfikuj połączenie** . Kliknij przycisk **Dalej**.  
  
    ![Pomyślnie zweryfikowano połączenie](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image27.png)
7. Otwórz listę rozwijaną **Parametry połączenia zdalnego** w obszarze **SchoolContext** i wybierz parametry połączenia dla utworzonej bazy danych.
8. Wybierz pozycję **wykonaj migracje Code First (uruchamiaj przy uruchamianiu aplikacji)** .
9. Usuń zaznaczenie pola **Użyj tych parametrów połączenia w czasie wykonywania** dla **UserContext (DefaultConnection)** , ponieważ ta aplikacja nie korzysta z bazy danych członkostwa.   
  
    ![Karta Ustawienia](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image28.png)
10. Kliknij przycisk **Dalej**.
11. Na karcie **Podgląd** kliknij pozycję **Rozpocznij podgląd**.  
  
    ![Przycisk StartPreview na karcie Podgląd](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image29.png)  
  
    Na karcie zostanie wyświetlona lista plików, które zostaną skopiowane na serwer programu. Wyświetlanie wersji zapoznawczej nie jest wymagane do opublikowania aplikacji, ale jest przydatną funkcją. W takim przypadku nie trzeba wykonywać żadnych czynności z listy wyświetlanych plików. Przy następnym wdrożeniu tej aplikacji na tej liście będą znajdować się tylko te pliki, które uległy zmianie.  
  
    ![Dane wyjściowe pliku StartPreview](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image30.png)
12. Kliknij przycisk **Publikuj**.  
    Program Visual Studio rozpoczyna proces kopiowania plików na serwer systemu Windows Azure.
13. W oknie **danych wyjściowych** znajdują się informacje o wykonywaniu akcji wdrażania i raporty o pomyślnym zakończeniu wdrożenia.  
  
    ![Pomyślne wdrożenie raportowania okna danych wyjściowych](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image31.png)
14. Po pomyślnym wdrożeniu domyślna przeglądarka automatycznie otwiera adres URL wdrożonej witryny sieci Web.  
    Utworzona aplikacja jest teraz uruchomiona w chmurze. Kliknij kartę studenci.  
  
    ![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image32.png)

W tym momencie baza danych *SchoolContext* została utworzona w Azure SQL Database systemu Windows, ponieważ wybrano **Polecenie Execute migracje Code First (uruchamiany przy uruchamianiu aplikacji)** . Plik *Web. config* w wdrożonej witrynie sieci Web został zmieniony tak, aby inicjator [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) był uruchamiany podczas pierwszego odczytywania lub zapisywania danych w bazie danych (co się stało po wybraniu karty **uczniów** ):

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image33.png)

Proces wdrażania również utworzył nowe parametry połączenia *(SchoolContext\_DatabasePublish*) dla migracje Code First, które mają być używane do aktualizowania schematu bazy danych i wypełniania bazy danych.

![Database_Publish parametry połączenia](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image34.png)

Parametry połączenia *DefaultConnection* są przeznaczone dla bazy danych członkostwa (której nie używamy w tym samouczku). Parametry połączenia *SchoolContext* są przeznaczone dla bazy danych ContosoUniversity.

Wdrożoną wersję pliku Web. config można znaleźć na własnym komputerze w *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. Możesz uzyskać dostęp do wdrożonego pliku *Web. config* przy użyciu protokołu FTP. Aby uzyskać instrukcje, zobacz [ASP.NET Web Deployment using Visual Studio: wdrażanie aktualizacji kodu](../../../../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md). Postępuj zgodnie z instrukcjami, które zaczynają się od "Aby użyć narzędzia FTP, potrzebne są trzy rzeczy: adres URL FTP, nazwa użytkownika i hasło".

> [!NOTE]
> Aplikacja sieci Web nie implementuje zabezpieczeń, dlatego każda osoba, która odnajdzie adres URL, może zmienić dane. Aby uzyskać instrukcje dotyczące zabezpieczania witryny sieci Web, zobacz [wdrażanie aplikacji secure ASP.NET MVC z członkostwem, uwierzytelnianiem OAuth i SQL Database w witrynie sieci Web systemu Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Można uniemożliwić innym osobom korzystanie z witryny przy użyciu portal zarządzania platformy Windows Azure lub **Eksplorator serwera** w programie Visual Studio, aby zatrzymać lokację.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image35.png)

## <a name="code-first-initializers"></a>Inicjatory Code First

W sekcji Deployment (wdrażanie) wykorzystano inicjatora [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) . Code First udostępnia również inne inicjatory, których można użyć, w tym [CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (wartość domyślna), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) i [DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx). Inicjator `DropCreateAlways` może być przydatny do ustawiania warunków dla testów jednostkowych. Możesz również napisać własne inicjatory i można wywołać inicjalizację jawnie, jeśli nie chcesz czekać, aż aplikacja odczyta lub zapisze w bazie danych. Aby zapoznać się ze szczegółowym opisem inicjatorów, zobacz rozdział 6 [Entity Framework programowania w książce: Code First](http://shop.oreilly.com/product/0636920022220.do) przez Julie Lerman i Rowan Miller.

## <a name="summary"></a>Podsumowanie

W tym samouczku pokazano, jak utworzyć model danych i zaimplementować podstawowe funkcje CRUD, sortowania, filtrowania, stronicowania i grupowania. W następnym samouczku zobaczysz bardziej zaawansowane tematy, rozszerzając model danych.

Linki do innych zasobów Entity Framework można znaleźć w [mapie zawartości dostęp do danych ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Poprzednie](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
> [dalej](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)

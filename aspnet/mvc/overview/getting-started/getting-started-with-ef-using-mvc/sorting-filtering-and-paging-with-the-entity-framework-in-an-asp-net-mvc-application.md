---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Samouczek: Dodawanie sortowania, filtrowania i stronicowania za pomocą Entity Framework w aplikacji ASP.NET MVC | Microsoft Docs'
author: tdykstra
description: W tym samouczku dodasz funkcje sortowania, filtrowania i stronicowania na stronie indeksu **uczniów** . Możesz również utworzyć prostą stronę grupowania.
ms.author: riande
ms.date: 01/14/2019
ms.assetid: d5723e46-41fe-4d09-850a-e03b9e285bfa
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: d9fadc12aa83a8095f364cf39e5376243a7d0670
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616040"
---
# <a name="tutorial-add-sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Samouczek: Dodawanie sortowania, filtrowania i stronicowania za pomocą Entity Framework w aplikacji ASP.NET MVC

W [poprzednim samouczku](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)zaimplementowano zestaw stron sieci Web dla podstawowych operacji CRUD dla jednostek `Student`. W tym samouczku dodasz funkcje sortowania, filtrowania i stronicowania na stronie indeksu **uczniów** . Możesz również utworzyć prostą stronę grupowania.

Na poniższej ilustracji przedstawiono, jak będzie wyglądać strona po zakończeniu. Nagłówki kolumn to linki, które użytkownik może kliknąć, aby posortować według tej kolumny. Wielokrotne kliknięcie nagłówka kolumny powoduje przełączenie między rosnącą i malejącą kolejnością sortowania.

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

W tym samouczku zostaną wykonane następujące czynności:

> [!div class="checklist"]
> * Dodaj linki sortowania kolumn
> * Dodawanie pola wyszukiwania
> * Dodaj stronicowanie
> * Tworzenie strony informacje

## <a name="prerequisites"></a>Wymagania wstępne

* [Implementowanie podstawowych funkcji CRUD](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)

## <a name="add-column-sort-links"></a>Dodaj linki sortowania kolumn

Aby dodać sortowanie na stronie indeksu ucznia, należy zmienić metodę `Index` kontrolera `Student` i dodać kod do widoku indeksu `Student`.

### <a name="add-sorting-functionality-to-the-index-method"></a>Dodawanie funkcji sortowania do metody index

- W *Controllers\StudentController.cs*Zastąp metodę `Index` następującym kodem:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Ten kod otrzymuje parametr `sortOrder` z ciągu zapytania w adresie URL. Wartość ciągu zapytania jest dostarczana przez ASP.NET MVC jako parametr do metody akcji. Parametr jest ciągiem "name" lub "date", opcjonalnie poprzedzonym znakiem podkreślenia i ciągiem "DESC" w celu określenia kolejności malejącej. Domyślna kolejność sortowania to Ascending.

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

Metoda używa [LINQ to Entities](/dotnet/framework/data/adonet/ef/language-reference/linq-to-entities) , aby określić kolumnę, według której ma zostać wykonane sortowanie. Kod tworzy zmienną <xref:System.Linq.IQueryable%601> przed instrukcją `switch`, modyfikuje ją w instrukcji `switch` i wywołuje metodę `ToList` po instrukcji `switch`. Gdy tworzysz i modyfikujesz zmienne `IQueryable`, żadne zapytanie nie jest wysyłane do bazy danych. Zapytanie nie jest wykonywane do momentu przekonwertowania obiektu `IQueryable` do kolekcji przez wywołanie metody, takiej jak `ToList`. W związku z tym ten kod skutkuje pojedynczym zapytaniem, które nie jest wykonywane do momentu instrukcji `return View`.

Alternatywą dla pisania różnych instrukcji LINQ dla każdej kolejności sortowania jest możliwość dynamicznego tworzenia instrukcji LINQ. Aby uzyskać informacje na temat dynamicznego LINQ, zobacz [dynamiczny LINQ](https://go.microsoft.com/fwlink/?LinkID=323957).

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Dodawanie hiperłączy nagłówka kolumny do widoku indeksu ucznia

1. W *Views\Student\Index.cshtml*zastąp `<tr>` i `<th>` elementów dla wiersza nagłówka z wyróżnionym kodem:

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

   Ten kod używa informacji we właściwościach `ViewBag`, aby skonfigurować hiperłącza z odpowiednimi wartościami ciągu zapytania.

2. Uruchom stronę i kliknij nagłówki kolumny **nazwisko** i **Data rejestracji** , aby sprawdzić, czy sortowanie działa.

   Po **kliknięciu nagłówka nazwisko** uczniowie będą wyświetlane w kolejności malejących nazwisk.

## <a name="add-a-search-box"></a>Dodawanie pola wyszukiwania

Aby dodać filtrowanie do strony indeksu uczniów, należy dodać pole tekstowe i przycisk Prześlij do widoku i wprowadzić odpowiednie zmiany w metodzie `Index`. Pole tekstowe umożliwia wprowadzenie ciągu do wyszukania w polach Imię i nazwisko.

### <a name="add-filtering-functionality-to-the-index-method"></a>Dodawanie funkcji filtrowania do metody index

- W *Controllers\StudentController.cs*Zastąp metodę `Index` następującym kodem (zmiany są wyróżnione):

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

Kod dodaje do metody `Index` parametr `searchString`. Wartość ciągu wyszukiwania jest odbierana z pola tekstowego, które zostanie dodane do widoku indeksu. Dodaje również klauzulę `where` do instrukcji LINQ, która wybiera tylko uczniów, których imię lub nazwisko zawiera ciąg wyszukiwania. Instrukcja, która dodaje klauzulę <xref:System.Linq.Queryable.Where%2A>, jest wykonywana tylko wtedy, gdy istnieje wartość do wyszukania.

> [!NOTE]
> W wielu przypadkach można wywołać tę samą metodę w ramach zestawu jednostek Entity Framework lub jako metodę rozszerzenia w kolekcji w pamięci. Wyniki są zwykle takie same, ale w niektórych przypadkach mogą być różne.
>
> Na przykład .NET Framework implementacja metody `Contains` zwraca wszystkie wiersze w przypadku przekazania do niego pustego ciągu, ale dostawca Entity Framework dla SQL Server Compact 4,0 zwraca zero wierszy dla pustych ciągów. W związku z tym kod w przykładzie (umieszczenie instrukcji `Where` wewnątrz instrukcji `if`) gwarantuje, że te same wyniki są uzyskiwane dla wszystkich wersji SQL Server. Ponadto .NET Framework implementacja metody `Contains` domyślnie wykonuje porównanie z uwzględnieniem wielkości liter, ale Entity Framework dostawca SQL Server domyślnie wykonuje porównania bez uwzględniania wielkości liter. W związku z tym, wywołując metodę `ToUpper`, aby test jawnie nie uwzględniał wielkości liter, nie zmienia się w przypadku późniejszej zmiany kodu w celu użycia repozytorium, które zwróci `IEnumerable` kolekcji zamiast obiektu `IQueryable`. (W przypadku wywołania metody `Contains` w kolekcji `IEnumerable` zostanie .NET Framework implementacja. po wywołaniu dla obiektu `IQueryable` zostanie wykorzystana implementacja dostawcy bazy danych).
>
> Obsługa wartości null może być również różna dla różnych dostawców baz danych lub jeśli używasz obiektu `IQueryable` w porównaniu z używaniem `IEnumerable` kolekcji. Na przykład w niektórych scenariuszach warunek `Where`, taki jak `table.Column != 0`, może nie zwracać kolumn, które mają `null` jako wartość. Domyślnie program Dr generuje dodatkowe operatory SQL, aby zapewnić równość między wartościami null w bazie danych, tak jak to działa w pamięci, ale można ustawić flagę [UseDatabaseNullSemantics](https://docs.microsoft.com/dotnet/api/system.data.entity.infrastructure.dbcontextconfiguration.usedatabasenullsemantics) w Ef6 lub wywołać metodę [UseRelationalNulls](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.infrastructure.relationaldbcontextoptionsbuilder-2.userelationalnulls) w EF Core, aby skonfigurować to zachowanie.

### <a name="add-a-search-box-to-the-student-index-view"></a>Dodawanie pola wyszukiwania do widoku indeksu ucznia

1. W *Views\Student\Index.cshtml*, Dodaj wyróżniony kod bezpośrednio przed otwierającym tagiem `table`, aby utworzyć podpis, pole tekstowe i przycisk **wyszukiwania** .

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=4-11)]

2. Uruchom stronę, wprowadź ciąg wyszukiwania, a następnie kliknij przycisk **Wyszukaj** , aby sprawdzić, czy filtrowanie działa.

   Zwróć uwagę, że adres URL nie zawiera ciągu wyszukiwania "a", co oznacza, że jeśli utworzysz zakładkę na tej stronie, nie otrzymasz filtrowanej listy przy użyciu zakładki. Dotyczy to również linków do sortowania kolumn, ponieważ sortuje całą listę. Zmienisz przycisk **wyszukiwania** , aby używać ciągów zapytania dla kryteriów filtrowania w dalszej części tego samouczka.

## <a name="add-paging"></a>Dodaj stronicowanie

Aby dodać stronicowanie do strony indeksu uczniów, Zacznij od zainstalowania pakietu NuGet **PagedList. MVC** . Następnie wprowadzisz dodatkowe zmiany w metodzie `Index` i dodasz linki stronicowania do widoku `Index`. **PagedList. MVC** to jeden z wielu dobrych pakietów stronicowania i sortowania dla ASP.NET MVC, a jego użycie w tym miejscu jest przeznaczone tylko jako przykład, a nie jako zalecenia dotyczące innych opcji.

### <a name="install-the-pagedlistmvc-nuget-package"></a>Zainstaluj pakiet NuGet PagedList. MVC

Pakiet NuGet **PagedList. MVC** automatycznie instaluje pakiet **PagedList** jako zależność. Pakiet **PagedList** instaluje `PagedList` typ kolekcji i metody rozszerzające dla kolekcji `IQueryable` i `IEnumerable`. Metody rozszerzające umożliwiają utworzenie pojedynczej strony danych w kolekcji `PagedList` z `IQueryable` lub `IEnumerable`, a kolekcja `PagedList` zawiera kilka właściwości i metod, które ułatwiają stronicowanie. Pakiet **PagedList. MVC** instaluje pomocnika stronicowania, który wyświetla przyciski stronicowania.

1. W menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet** , a następnie **konsolę Menedżera pakietów**.

2. W oknie **konsola Menedżera pakietów** upewnij się, że **źródło pakietu** to **NuGet.org** , a **Projekt domyślny** to **ContosoUniversity**, a następnie wprowadź następujące polecenie:

   ```text
   Install-Package PagedList.Mvc
   ```

3. Skompiluj projekt.

### <a name="add-paging-functionality-to-the-index-method"></a>Dodawanie funkcji stronicowania do metody index

1. W *Controllers\StudentController.cs*dodaj instrukcję `using` dla przestrzeni nazw `PagedList`:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

2. Zastąp metodę `Index` poniższym kodem:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=1,3,7-16,41-43)]

   Ten kod dodaje parametr `page`, bieżący parametr kolejności sortowania i bieżący parametr filtru do sygnatury metody:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

   Gdy strona jest wyświetlana po raz pierwszy, lub jeśli użytkownik nie kliknął linku stronicowania ani sortowania, wszystkie parametry mają wartość null. Jeśli kliknięto łącze stronicowania, zmienna `page` zawiera numer strony do wyświetlenia.

   Właściwość `ViewBag` zapewnia widok z bieżącą kolejnością sortowania, ponieważ musi być uwzględniony w łączach stronicowania, aby zachować porządek sortowania w tym samym czasie podczas stronicowania:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

   Inna właściwość, `ViewBag.CurrentFilter`, udostępnia widok z bieżącym ciągiem filtru. Ta wartość musi być uwzględniona w łączach stronicowania, aby zachować ustawienia filtru podczas stronicowania, i musi zostać przywrócone do pola tekstowego, gdy strona jest ponownie wyświetlana. Jeśli ciąg wyszukiwania zostanie zmieniony podczas stronicowania, należy zresetować stronę do 1, ponieważ nowy filtr może spowodować wyświetlenie różnych danych. Ciąg wyszukiwania jest zmieniany, gdy wartość zostanie wprowadzona w polu tekstowym, a przycisk Prześlij zostanie naciśnięty. W takim przypadku parametr `searchString` nie ma wartości null.

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

   Na końcu metody `ToPagedList` Metoda rozszerzenia w obiekcie Students `IQueryable` konwertuje zapytanie ucznia na jedną stronę uczniów w typie kolekcji, który obsługuje stronicowanie. Ta pojedyncza strona studentów jest następnie przenoszona do widoku:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

   Metoda `ToPagedList` przyjmuje numer strony. Dwa znaki zapytania reprezentują [operator łączenia wartości null](/dotnet/csharp/language-reference/operators/null-coalescing-operator). Operator łączenia wartości null definiuje wartość domyślną dla typu dopuszczającego wartość null; wyrażenie `(page ?? 1)` oznacza zwrócenie wartości `page`, jeśli ma wartość, lub zwraca wartość 1, jeśli `page` jest wartością null.

### <a name="add-paging-links-to-the-student-index-view"></a>Dodawanie linków stronicowania do widoku indeksu ucznia

1. W *Views\Student\Index.cshtml*Zastąp istniejący kod następującym kodem. Zmiany są wyróżnione.

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=1-3,6,9,14,17,24,30,55-56,58-59)]

   Instrukcja `@model` w górnej części strony określa, że widok pobiera teraz obiekt `PagedList` zamiast obiektu `List`.

   Instrukcja `using` dla `PagedList.Mvc` daje dostęp do pomocnika MVC dla przycisków stronicowania.

   Kod używa przeciążenia elementu [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) , który umożliwia określenie [FormMethod. Get](/previous-versions/aspnet/dd460179(v=vs.100)).

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

   Wartość domyślna [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) przesyła dane formularza z wpisem, co oznacza, że parametry są przesyłane w treści wiadomości HTTP, a nie w adresie URL jako ciągi zapytań. Po określeniu protokołu HTTP GET dane formularza są przesyłane w adresie URL jako ciągi zapytań, co umożliwia użytkownikom tworzenie zakładek w adresie URL. [Wskazówki W3C dotyczące korzystania z protokołu HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) zalecają korzystanie z usługi Get, gdy akcja nie spowoduje aktualizacji.

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

2. Uruchom stronę.

   Kliknij linki stronicowania w różnych kolejności sortowania, aby upewnić się, że stronicowanie działa. Następnie wprowadź ciąg wyszukiwania i spróbuj ponownie wykonać stronicowanie, aby sprawdzić, czy stronicowanie działa prawidłowo z sortowaniem i filtrowaniem.

## <a name="create-an-about-page"></a>Tworzenie strony informacje

Na stronie informacje o witrynie sieci Web dla uniwersytetów firmy Contoso można wyświetlić liczbę studentów zarejestrowanych dla każdej daty rejestracji. Wymaga to grupowania i prostych obliczeń dla grup. Aby to osiągnąć, należy wykonać następujące czynności:

- Utwórz klasę modelu widoku dla danych, które należy przekazać do widoku.
- Zmodyfikuj metodę `About` na kontrolerze `Home`.
- Zmodyfikuj widok `About`.

### <a name="create-the-view-model"></a>Tworzenie modelu widoku

Utwórz folder *modele widoków* w folderze projektu. W tym folderze Dodaj plik klasy *EnrollmentDateGroup.cs* i Zastąp kod szablonu następującym kodem:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>Modyfikowanie kontrolera macierzystego

1. W *HomeController.cs*Dodaj następujące instrukcje `using` w górnej części pliku:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

2. Dodaj zmienną klasy dla kontekstu bazy danych bezpośrednio po otwierającym nawiasie klamrowym dla klasy:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

3. Zastąp metodę `About` poniższym kodem:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

   Instrukcja LINQ grupuje jednostki uczniów według daty rejestracji, oblicza liczbę jednostek w każdej grupie i zapisuje wyniki w kolekcji `EnrollmentDateGroup` widoku obiektów modelu.

4. Dodaj metodę `Dispose`:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>Modyfikowanie widoku informacje

1. Zastąp kod w pliku *Views\Home\About.cshtml* następującym kodem:

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

2. Uruchom aplikację i kliknij łącze **informacje** .

   Liczba studentów dla każdej daty rejestracji zostanie wyświetlona w tabeli.

   ![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="get-the-code"></a>Uzyskiwanie kodu

[Pobierz ukończony projekt](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Dodatkowe zasoby

Linki do innych zasobów Entity Framework można znaleźć w temacie [ASP.NET Data Access — zalecane zasoby](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Następne kroki

W tym samouczku zostaną wykonane następujące czynności:

> [!div class="checklist"]
> * Dodaj linki sortowania kolumn
> * Dodawanie pola wyszukiwania
> * Dodaj stronicowanie
> * Tworzenie strony informacje

Przejdź do następnego artykułu, aby dowiedzieć się, jak korzystać z odporności połączeń i przechwycenia poleceń.
> [!div class="nextstepaction"]
> [Odporność połączeń i przechwycenie poleceń](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)

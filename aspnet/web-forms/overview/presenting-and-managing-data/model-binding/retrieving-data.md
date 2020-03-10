---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: Pobieranie i wyświetlanie danych przy użyciu powiązania modelu i formularzy sieci Web | Microsoft Docs
author: Rick-Anderson
description: Ta seria samouczków pokazuje podstawowe aspekty używania powiązania modelu z projektem formularzy sieci Web ASP.NET. Powiązanie modelu sprawia, że interakcje danych są bardziej proste-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: 81cca22cb4752d071d2a68986ae9ac2bed737594
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78640197"
---
# <a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a>Pobieranie i wyświetlanie danych przy użyciu powiązania modelu i formularzy sieci Web

> Ta seria samouczków pokazuje podstawowe aspekty używania powiązania modelu z projektem formularzy sieci Web ASP.NET. Powiązanie modelu sprawia, że interakcje danych są bardziej proste, niż w przypadku obiektów źródła danych (np. ObjectDataSource lub kontrolki SqlDataSource). Ta seria rozpoczyna się od materiału wstępnego i przenosi do bardziej zaawansowanych koncepcji w kolejnych samouczkach.
> 
> Wzorzec powiązania modelu współpracuje z dowolnymi technologiami dostępu do danych. W tym samouczku będziesz używać Entity Framework, ale możesz korzystać z najpopularniejszej technologii dostępu do danych. Z poziomu formantu serwera powiązanego z danymi, takiego jak formant GridView, ListView, DetailsView lub FormView, należy określić nazwy metod, które mają być używane do wybierania, aktualizowania, usuwania i tworzenia danych. W tym samouczku określisz wartość dla operacji SelectMethod. 
> 
> W ramach tej metody należy podać logikę pobierania danych. W następnym samouczku ustawisz wartości dla UpdateMethod, DeleteMethod i InsertMethod.
>
> Możesz [pobrać](https://go.microsoft.com/fwlink/?LinkId=286116) kompletny projekt w programie C# lub Visual Basic. Kod do pobrania współpracuje z programem Visual Studio 2012 lub nowszym. Używa szablonu programu Visual Studio 2012, który jest nieco inny niż szablon programu Visual Studio 2017 przedstawiony w tym samouczku.
> 
> Samouczek dotyczący uruchamiania aplikacji w programie Visual Studio. Możesz również wdrożyć aplikację w dostawcy hostingu i udostępnić ją za pośrednictwem Internetu. Firma Microsoft oferuje bezpłatny hosting w sieci Web dla maksymalnie 10 witryn sieci Web w  
> [bezpłatne konto wersji próbnej platformy Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604). Aby uzyskać informacje na temat sposobu wdrażania projektu sieci Web programu Visual Studio w celu Azure App Service Web Apps, zobacz [wdrażanie w sieci web ASP.NET za pomocą programu Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) Series. Ten samouczek pokazuje również, jak używać migracje Code First platformy Entity Framework do wdrażania bazy danych SQL Server do Azure SQL Database.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
> 
> - Microsoft Visual Studio 2017 lub Microsoft Visual Studio Community 2017
>   
> Ten samouczek działa również z programem Visual Studio 2012 i Visual Studio 2013, ale istnieją pewne różnice w interfejsie użytkownika i szablonie projektu.

## <a name="what-youll-build"></a>Co będziesz kompilować

W tym samouczku przedstawiono następujące instrukcje:

* Kompiluj obiekty danych, które odzwierciedlają Uniwersytet z uczniami zarejestrowanymi w kursach
* Tworzenie tabel bazy danych z obiektów
* Wypełnianie bazy danych danymi testowymi
* Wyświetlanie danych w formularzu sieci Web

## <a name="create-the-project"></a>Tworzenie projektu

1. W programie Visual Studio 2017 Utwórz projekt **aplikacji sieci Web ASP.NET (.NET Framework)** o nazwie **ContosoUniversityModelBinding**.

   ![Utwórz projekt](retrieving-data/_static/image19.png)

2. Kliknij przycisk **OK**. Zostanie wyświetlone okno dialogowe z wybranym szablonem.

   ![Wybieranie formularzy sieci Web](retrieving-data/_static/image3.png)

3. Wybierz szablon **formularzy sieci Web** . 

4. W razie potrzeby zmień uwierzytelnianie na **konta poszczególnych użytkowników**. 

5. Wybierz przycisk **OK**, aby utworzyć projekt.

## <a name="modify-site-appearance"></a>Modyfikuj wygląd witryny

   Wprowadź kilka zmian, aby dostosować wygląd witryny. 
   
   1. Otwórz plik site. Master.
   
   2. Zmień tytuł w taki sposób, aby wyświetlał firmę **Contoso University** , a nie **moją aplikację ASP.NET**.

      [!code-aspx-csharp[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

   3. Zmień tekst nagłówka z **nazwy aplikacji** na **Uniwersytet firmy Contoso**.

      [!code-aspx-csharp[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

   4. Zmień linki nagłówka nawigacji odpowiednio do odpowiedniej lokacji. 
   
      Usuń linki do programu i **skontaktuj się** z nim **, a zamiast** tego Połącz się ze stroną **uczniów** , którą utworzysz.

      [!code-aspx-csharp[Main](retrieving-data/samples/sample3.aspx)]

   5. Zapisz witrynę site. Master.

## <a name="add-a-web-form-to-display-student-data"></a>Dodaj formularz sieci Web, aby wyświetlić dane ucznia

   1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, wybierz polecenie **Dodaj** , a następnie **nowy element**. 
   
   2. W oknie dialogowym **Dodaj nowy element** wybierz **formularz sieci Web z szablonem strony głównej** i nadaj mu nazwę **Students. aspx**.

      ![Utwórz stronę](retrieving-data/_static/image5.png)

   3. Wybierz pozycję **Dodaj**.
   
   4. Na stronie wzorcowej formularza sieci Web wybierz pozycję **site. Master**.
   
   5. Kliknij przycisk **OK**.

## <a name="add-the-data-model"></a>Dodawanie modelu danych

W folderze **modele** Dodaj klasę o nazwie **UniversityModels.cs**.

   1. Kliknij prawym przyciskiem myszy pozycję **modele**, wybierz pozycję **Dodaj**, a następnie pozycję **nowy element**. Zostanie wyświetlone okno dialogowe **Dodawanie nowego elementu**.

   2. W menu nawigacji po lewej stronie wybierz pozycję **Code**, a następnie **Class**.

      ![Utwórz klasę modelu](retrieving-data/_static/image20.png)

   3. Nadaj klasie nazwę **UniversityModels.cs** i wybierz pozycję **Dodaj**.

      W tym pliku Zdefiniuj klasy `SchoolContext`, `Student`, `Enrollment`i `Course` w następujący sposób:

      [!code-csharp[Main](retrieving-data/samples/sample4.cs)]

      Klasa `SchoolContext` pochodzi od `DbContext`, która zarządza połączeniem bazy danych i zmianami danych.

      W klasie `Student` Zwróć uwagę na atrybuty zastosowane do właściwości `FirstName`, `LastName`i `Year`. Ten samouczek używa tych atrybutów do sprawdzania poprawności danych. Aby uprościć kod, tylko te właściwości są oznaczone atrybutami walidacji danych. W rzeczywistym projekcie należy zastosować atrybuty walidacji do wszystkich właściwości wymagających walidacji.

   4. Save UniversityModels.cs.

## <a name="set-up-the-database-based-on-classes"></a>Skonfiguruj bazę danych na podstawie klas

Ten samouczek używa [migracje Code First](https://docs.microsoft.com/ef/ef6/modeling/code-first/migrations/) do tworzenia obiektów i tabel bazy danych. Te tabele przechowują informacje dotyczące uczniów i ich kursów.

   1. Wybierz kolejno pozycje **narzędzia** > **menedżer pakietów NuGet** > **konsola Menedżera pakietów**.

   2. W **konsoli Menedżera pakietów**Uruchom następujące polecenie:  
      `enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

      Jeśli polecenie zostanie wykonane pomyślnie, zostanie wyświetlony komunikat z informacją o włączeniu migracji.

      ![Włącz migracje](retrieving-data/_static/image8.png)

      Zauważ, że plik o nazwie *Configuration.cs* został utworzony. Klasa `Configuration` ma metodę `Seed`, która umożliwia wstępne wypełnianie tabel bazy danych danymi testowymi.

## <a name="pre-populate-the-database"></a>Wstępne wypełnianie bazy danych

   1. Otwórz Configuration.cs.
   
   2. Dodaj następujący kod do metody `Seed`: Ponadto Dodaj instrukcję `using` dla przestrzeni nazw `ContosoUniversityModelBinding. Models`.

      [!code-csharp[Main](retrieving-data/samples/sample5.cs)]

   3. Zapisz Configuration.cs.

   4. W konsoli Menedżera pakietów Uruchom polecenie **Dodaj wstępną migrację**polecenia.

   5. Uruchom polecenie **Update-Database**.

      Jeśli podczas uruchamiania tego polecenia zostanie wyświetlony wyjątek, wartości `StudentID` i `CourseID` mogą różnić się od wartości metody `Seed`. Otwórz te tabele bazy danych i Znajdź istniejące wartości dla `StudentID` i `CourseID`. Dodaj te wartości do kodu umożliwiającego wypełnianie tabeli `Enrollments`.

## <a name="add-a-gridview-control"></a>Dodaj kontrolkę GridView

Przy użyciu wypełnionych danych bazy danych możesz teraz przystąpić do pobierania tych danych i wyświetlania ich. 

1. Otwórz uczniów. aspx.

2. Znajdź `MainContent` symbol zastępczy. W tym symbolu zastępczym Dodaj kontrolkę **GridView** , która zawiera ten kod.

   [!code-aspx-csharp[Main](retrieving-data/samples/sample6.aspx)]

   Kwestie do uwagi:
   * Zwróć uwagę na wartość ustawioną dla właściwości `SelectMethod` w elemencie GridView. Ta wartość określa metodę używaną do pobierania danych GridView, które tworzysz w następnym kroku. 
   
   * Właściwość `ItemType` jest ustawiona na utworzoną wcześniej klasy `Student`. To ustawienie umożliwia odwołanie do właściwości klasy w znaczniku. Na przykład Klasa `Student` ma kolekcję o nazwie `Enrollments`. Możesz użyć `Item.Enrollments` do pobrania tej kolekcji, a następnie użyć [składni LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) , aby pobrać sumę kredytów zarejestrowanej przez studenta.
   
3. Zapisz uczniów. aspx.

## <a name="add-code-to-retrieve-data"></a>Dodawanie kodu w celu pobierania danych

   W pliku student. aspx z kodem należy dodać metodę określoną dla wartości `SelectMethod`. 
   
   1. Otwórz Students.aspx.cs.
   
   2. Dodaj `using` instrukcje dla przestrzeni nazw `ContosoUniversityModelBinding. Models` i `System.Data.Entity`.

      [!code-csharp[Main](retrieving-data/samples/sample7.cs)]

   3. Dodaj metodę określoną dla `SelectMethod`:

      [!code-csharp[Main](retrieving-data/samples/sample8.cs)]

      Klauzula `Include` podnosi wydajność zapytań, ale nie jest wymagana. Bez klauzuli `Include` dane są pobierane przy użyciu [*ładowania opóźnionego*](https://en.wikipedia.org/wiki/Lazy_loading), które polega na wysyłaniu oddzielnego zapytania do bazy danych za każdym razem, gdy są pobierane powiązane dane. Z klauzulą `Include` dane są pobierane przy użyciu *ładowania eager*, co oznacza, że pojedyncze zapytanie bazy danych pobiera wszystkie powiązane dane. Jeśli powiązane dane nie są używane, ładowanie eager jest mniej wydajne, ponieważ jest pobieranych więcej danych. Jednak w takim przypadku ładowanie eager zapewnia najlepszą wydajność, ponieważ dla każdego rekordu są wyświetlane powiązane dane.

      Aby uzyskać więcej informacji na temat zagadnień dotyczących wydajności podczas ładowania powiązanych danych, zobacz sekcję **opóźnione, Eager i jawne ładowanie powiązanych danych** w sekcji [odczytywanie powiązanych danych za pomocą Entity Framework w artykule ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) .

      Domyślnie dane są sortowane według wartości właściwości oznaczonej jako klucz. Można dodać klauzulę `OrderBy`, aby określić inną wartość sortowania. W tym przykładzie domyślna właściwość `StudentID` jest używana do sortowania. W artykule [sortowanie, stronicowanie i filtrowanie danych](sorting-paging-and-filtering-data.md) użytkownik ma możliwość wybrania kolumny do sortowania.
 
   4. Zapisz Students.aspx.cs.

## <a name="run-your-application"></a>Uruchamianie aplikacji 

Uruchom aplikację sieci Web (**F5**) i przejdź do strony **uczniów** , która wyświetla następujące elementy:

   ![Pokaż dane](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a>Automatyczne generowanie metod powiązania modelu

Podczas pracy z tą serią samouczków możesz po prostu skopiować kod z samouczka do projektu. Jednak jedną wadą tego podejścia jest to, że użytkownik może nie być świadomy funkcji dostarczonej przez program Visual Studio w celu automatycznego generowania kodu dla metod powiązania modelu. Podczas pracy nad własnymi projektami automatyczne generowanie kodu może zaoszczędzić czas i pomóc w uzyskaniu tego, jak zaimplementować operację. W tej sekcji opisano funkcję automatycznego generowania kodu. Ta sekcja jest tylko informacyjna i nie zawiera kodu, który jest wymagany do zaimplementowania w projekcie. 

Podczas ustawiania wartości właściwości `SelectMethod`, `UpdateMethod`, `InsertMethod`lub `DeleteMethod` w kodzie znaczników można wybrać opcję **Utwórz nową metodę** .

![Tworzenie metody](retrieving-data/_static/image18.png)

Program Visual Studio nie tylko tworzy metodę w kodzie związanym z prawidłowym podpisem, ale również generuje kod implementacji do wykonania operacji. Jeśli najpierw ustawisz właściwość `ItemType` przed użyciem funkcji automatycznego generowania kodu, wygenerowany kod używa tego typu dla operacji. Na przykład podczas ustawiania właściwości `UpdateMethod` następujący kod jest generowany automatycznie:

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

Ponownie ten kod nie musi być dodany do projektu. W następnym samouczku zostaną zaimplementowane metody aktualizowania, usuwania i dodawania nowych danych.

## <a name="summary"></a>Podsumowanie

W tym samouczku utworzono klasy modelu danych i Wygenerowano bazę danych z tych klas. Tabele bazy danych zostały wypełnione danymi testowymi. Użyto powiązania modelu do pobierania danych z bazy danych, a następnie wyświetlania danych w widoku GridView.

W następnym [samouczku](updating-deleting-and-creating-data.md) w tej serii będziesz włączać aktualizowanie, usuwanie i tworzenie danych.

> [!div class="step-by-step"]
> [Dalej](updating-deleting-and-creating-data.md)

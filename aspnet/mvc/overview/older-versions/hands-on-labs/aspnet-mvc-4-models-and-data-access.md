---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: ASP.NET modelu MVC 4 i dostęp do danych | Microsoft Docs
author: rick-anderson
description: 'Uwaga: w tym ćwiczeniu praktycznym założono, że masz podstawową wiedzę na temat ASP.NET MVC. Jeśli przed nie korzystasz z ASP.NET MVC, zalecamy przejście do ASP.NET MVC 4...'
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: 90635b617930d0a9c126795f4c8790d542e33dc9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78560201"
---
# <a name="aspnet-mvc-4-models-and-data-access"></a>ASP.NET MVC 4 — modele i dostęp do danych

przez [zespół Camp sieci Web](https://twitter.com/webcamps)

[Pobierz zestaw szkoleniowy dla sieci Web Camp](https://aka.ms/webcamps-training-kit)

W ramach tego ćwiczenia praktycznego założono, że masz podstawową wiedzę na temat **ASP.NET MVC**. Jeśli wcześniej nie korzystasz z **ASP.NET MVC** , zalecamy przechodzenie przez **ASP.NETe podstawowe wskazówki dotyczące MVC 4** .

To laboratorium przeprowadzi Cię przez ulepszenia i nowe funkcje opisane wcześniej przez zastosowanie drobnych zmian do przykładowej aplikacji sieci Web, która znajduje się w folderze źródłowym.

> [!NOTE]
> Wszystkie przykładowe kod i fragmenty kodu są zawarte w zestawie szkoleń w sieci Web Camp, które są dostępne w [wersjach Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Projekt specyficzny dla tego laboratorium jest dostępny w [modelach ASP.NET MVC 4 i dostęp do danych](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).

W środowisku **ASP.NET MVC** — Omówienie praktycznego przekazywania danych z kontrolerów do szablonów widoków. Jednak w celu utworzenia rzeczywistej aplikacji sieci Web warto użyć rzeczywistej bazy danych.

W ramach tego praktycznego laboratorium pokazano, jak używać aparatu bazy danych w celu przechowywania i pobierania danych potrzebnych dla aplikacji do sklepu muzycznego. Aby to osiągnąć, należy rozpocząć od istniejącej bazy danych i utworzyć Entity Data Model od niej. W tym laboratorium spełnimy podejście **Database First** , a także podejście **Code First** .

Można jednak również użyć podejścia **model First** , utworzyć ten sam model przy użyciu narzędzi, a następnie wygenerować z niego bazę danych.

![Database First a Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First a Model First")

*Database First a Model First*

Po wygenerowaniu modelu wprowadzisz odpowiednie korekty w StoreController, aby udostępnić widoki magazynu z danymi pobranymi z bazy danych, zamiast korzystać z zakodowanych danych. Nie musisz wprowadzać żadnych zmian w szablonach widoku, ponieważ StoreController będzie zwracać ten sam modele widoków do szablonów widoków, chociaż dane będą pochodzić z bazy danych.

**Podejście Code First**

Podejście Code First umożliwia zdefiniowanie modelu na podstawie kodu bez generowania klas, które są ogólnie powiązane z platformą.

W kodzie najpierw obiekty modelu są definiowane za pomocą POCOs, &quot;zwykłych starych obiektów CLR&quot;. POCOs są prostymi klasami czystymi, które nie mają dziedziczenia i nie implementują interfejsów. Można automatycznie wygenerować z nich bazę danych lub użyć istniejącej bazy danych i wygenerować mapowanie klasy na podstawie kodu.

Zaletą tego podejścia jest to, że model pozostaje niezależny od struktury trwałości (w tym przypadku Entity Framework), ponieważ klasy POCOs nie są sprzężone z strukturą mapowania.

> [!NOTE]
> To laboratorium jest oparte na ASP.NET MVC 4 oraz wersji przykładowej aplikacji, która została dostosowana i zminimalizowana do dopasowania tylko do funkcji przedstawionych w tym środowisku.
> 
> Jeśli chcesz zapoznać się z samouczkiem dotyczącym całego **sklepu muzycznego** , możesz go znaleźć w [sklepie MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

Aby ukończyć to laboratorium, musisz mieć następujące elementy:

- [Microsoft Visual Studio Express 2012 dla sieci Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub nadrzędnych (Przeczytaj [dodatek A,](#AppendixA) Aby uzyskać instrukcje na temat sposobu jego instalacji).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Konfigurowanie

**Instalowanie fragmentów kodu**

Dla wygody większość kodu, który będzie zarządzany w tym laboratorium, jest dostępna jako fragmenty kodu programu Visual Studio. Aby zainstalować plik fragmentów kodu, uruchom **.\Source\Setup\CodeSnippets.VSI** .

Jeśli nie znasz fragmentów kodu Visual Studio Code i chcesz dowiedzieć się, jak z nich korzystać, możesz odwołać się do dodatku z tego dokumentu &quot;[dodatku C: Using fragmenty kodu](#AppendixC)&quot;.

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Symulacyjn

To laboratorium praktyczne obejmuje następujące ćwiczenia:

1. [Ćwiczenie 1: Dodawanie bazy danych](#Exercise1)
2. [Ćwiczenie 2: Tworzenie bazy danych przy użyciu Code First](#Exercise2)
3. [Ćwiczenie 3: wykonywanie zapytania dotyczącego bazy danych za pomocą parametrów](#Exercise3)

> [!NOTE]
> Każdemu ćwiczeniu towarzyszy folder **końcowy** zawierający rozwiązanie, które należy uzyskać po zakończeniu ćwiczenia. Tego rozwiązania można użyć jako przewodnika, jeśli potrzebujesz dodatkowej pomocy w pracy przez ćwiczenia.

Szacowany czas wykonywania tego laboratorium: **35 minut**.

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a>Ćwiczenie 1: Dodawanie bazy danych

W tym ćwiczeniu dowiesz się, jak dodać bazę danych z tabelami aplikacji MusicStore do rozwiązania, aby móc korzystać z jego danych. Gdy baza danych jest generowana przy użyciu modelu i dodawana do rozwiązania, należy zmodyfikować klasę StoreController, aby udostępnić szablon widoku z danymi pobranymi z bazy danych, zamiast korzystać z zakodowanych wartości.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a>Zadanie 1 — Dodawanie bazy danych

W tym zadaniu dodasz już utworzoną bazę danych z głównymi tabelami aplikacji MusicStore do rozwiązania.

1. Otwórz rozwiązanie **BEGIN** znajdujące się w lokalizacji **Source/Ex1-AddingADatabaseDBFirst/BEGIN/** folder.

   1. Przed kontynuowaniem musisz pobrać brakujące pakiety NuGet. W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.
   2. W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu. Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu. To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.
2. Dodaj plik bazy danych **MvcMusicStore** . W ramach tego praktycznego laboratorium zostanie użyta już utworzona baza danych o nazwie **MvcMusicStore. mdf**. Aby to zrobić, kliknij prawym przyciskiem myszy pozycję **aplikacja\_dane** , wskaż polecenie **Dodaj** , a następnie kliknij pozycję **istniejący element**. Przejdź do **\Source\Assets** i wybierz plik **MvcMusicStore. mdf** .

    ![Dodawanie istniejącego elementu](aspnet-mvc-4-models-and-data-access/_static/image2.png "Dodawanie istniejącego elementu")

    *Dodawanie istniejącego elementu*

    ![Plik bazy danych MvcMusicStore. mdf](aspnet-mvc-4-models-and-data-access/_static/image3.png "Plik bazy danych MvcMusicStore. mdf")

    *Plik bazy danych MvcMusicStore. mdf*

    Baza danych została dodana do projektu. Nawet jeśli baza danych znajduje się w rozwiązaniu, można wykonać zapytanie i zaktualizować ją, ponieważ była hostowana na innym serwerze bazy danych.

    ![Baza danych MvcMusicStore w Eksplorator rozwiązań](aspnet-mvc-4-models-and-data-access/_static/image4.png "Baza danych MvcMusicStore w Eksplorator rozwiązań")

    *Baza danych MvcMusicStore w Eksplorator rozwiązań*
3. Sprawdź połączenie z bazą danych. Aby to zrobić, kliknij dwukrotnie plik **MvcMusicStore. mdf** , aby nawiązać połączenie.

    ![Łączenie z MvcMusicStore. mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "Łączenie z MvcMusicStore. mdf")

    *Łączenie z MvcMusicStore. mdf*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a>Zadanie 2 — Tworzenie modelu danych

W tym zadaniu utworzysz model danych, który będzie mógł korzystać z bazy danych dodanej w poprzednim zadaniu.

1. Utwórz model danych, który będzie reprezentować bazę danych. W tym celu w Eksplorator rozwiązań kliknij prawym przyciskiem myszy folder **modele** , wskaż polecenie **Dodaj** , a następnie kliknij pozycję **nowy element**. W oknie dialogowym **Dodaj nowy element** wybierz szablon **danych** , a następnie element **ADO.NET Entity Data Model** . Zmień nazwę modelu danych na **StoreDB. edmx** , a następnie kliknij przycisk **Dodaj**.

    ![Dodawanie Entity Data Model ADO.NET StoreDB](aspnet-mvc-4-models-and-data-access/_static/image6.png "Dodawanie Entity Data Model ADO.NET StoreDB")

    *Dodawanie Entity Data Model ADO.NET StoreDB*
2. Zostanie wyświetlony **kreator Entity Data Model** . Ten Kreator przeprowadzi Cię przez proces tworzenia warstwy modelu. Ponieważ model powinien być tworzony na podstawie ostatnio dodanej bazy danych, wybierz pozycję **Generuj z bazy danych** i kliknij przycisk **dalej**.

    ![Wybieranie zawartości modelu](aspnet-mvc-4-models-and-data-access/_static/image7.png "Wybieranie zawartości modelu")

    *Wybieranie zawartości modelu*
3. Ponieważ generujesz model z bazy danych, musisz określić połączenie, które ma być używane. Kliknij pozycję **nowe połączenie**.
4. Wybierz **plik bazy danych Microsoft SQL Server** i kliknij przycisk **Kontynuuj**.

    ![Wybieranie źródła danych](aspnet-mvc-4-models-and-data-access/_static/image8.png "Wybieranie źródła danych")

    *Okno dialogowe Wybieranie źródła danych*
5. Kliknij przycisk **Przeglądaj** i wybierz bazę danych **MvcMusicStore. mdf** , która znajduje się w folderze **danych\_aplikacji** , a następnie kliknij przycisk **OK**.

    ![Właściwości połączenia](aspnet-mvc-4-models-and-data-access/_static/image9.png "Connection properties (Właściwości połączenia)")

    *Właściwości połączenia*
6. Wygenerowana Klasa powinna mieć taką samą nazwę jak parametry połączenia jednostki, więc Zmień jej nazwę na **MusicStoreEntities** i kliknij przycisk **dalej**.

    ![Wybieranie połączenia danych](aspnet-mvc-4-models-and-data-access/_static/image10.png "Wybieranie połączenia danych")

    *Wybieranie połączenia danych*
7. Wybierz obiekty bazy danych, które mają być używane. Ponieważ model jednostki będzie używać tylko tabel bazy danych, wybierz opcję **tabele** i upewnij się, że są także zaznaczone opcje **Uwzględnij kolumny klucza obcego w modelu** i **pluralize lub nazwom** . Zmień przestrzeń nazw modelu na **MvcMusicStore. model** i kliknij przycisk **Zakończ**.

    ![Wybieranie obiektów bazy danych](aspnet-mvc-4-models-and-data-access/_static/image11.png "Wybieranie obiektów bazy danych")

    *Wybieranie obiektów bazy danych*

    > [!NOTE]
    > Jeśli zostanie wyświetlone okno dialogowe ostrzeżenia o zabezpieczeniach, kliknij przycisk **OK** , aby uruchomić szablon i wygenerować klasy dla jednostek modelu.
8. Zostanie wyświetlony diagram jednostki dla bazy danych, podczas gdy zostanie utworzona oddzielna Klasa, która mapuje każdą tabelę na bazę danych. Na przykład tabela **albumy** będzie reprezentowana przez klasę **albumu** , gdzie każda kolumna w tabeli zostanie zamapowana na właściwość klasy. Pozwoli to na wykonywanie zapytań i pracy z obiektami, które reprezentują wiersze w bazie danych.

    ![Diagram jednostek](aspnet-mvc-4-models-and-data-access/_static/image12.png "Diagram jednostek")

    *Diagram jednostek*

    > [!NOTE]
    > Szablony T4 (. tt) uruchamiają kod w celu wygenerowania klas jednostek i zastąpią istniejące klasy o tej samej nazwie. W tym przykładzie klasy &quot;albumu&quot;, &quot;gatunek&quot; i &quot;artyści&quot; zostały zastąpione wygenerowanym kodem.

<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a>Zadanie 3 — Kompilowanie aplikacji

W tym zadaniu sprawdzisz, że mimo że generacja modelu usunęła klasy **album**, **gatunek** i model **wykonawcy** , projekt zostanie pomyślnie skompilowany przy użyciu nowych klas modelu danych.

1. Skompiluj projekt, wybierając element menu **kompilacja** , a następnie **Skompiluj MvcMusicStore**.

    ![Kompilowanie projektu](aspnet-mvc-4-models-and-data-access/_static/image13.png "Kompilowanie projektu")

    *Kompilowanie projektu*
2. Projekt zostanie pomyślnie skompilowany. Dlaczego nadal działa? Działa, ponieważ tabele bazy danych zawierają pola zawierające właściwości, które były używane w **albumie** i **gatunku**usuniętych klas.

    ![Kompilacja powiodła się](aspnet-mvc-4-models-and-data-access/_static/image14.png "Kompilacja powiodła się")

    *Kompilacja powiodła się*
3. Gdy Projektant wyświetla jednostki w formacie diagramu, są one w rzeczywistości C# klasą. Rozwiń węzeł **StoreDB. edmx** w Eksplorator rozwiązań a następnie pozycję **StoreDB.tt**, zobaczysz nowe wygenerowane jednostki.

    ![Wygenerowane pliki](aspnet-mvc-4-models-and-data-access/_static/image15.png "Wygenerowane pliki")

    *Wygenerowane pliki*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>Zadanie 4 — wykonywanie zapytania dotyczącego bazy danych

W tym zadaniu zostanie zaktualizowana Klasa StoreController, tak aby zamiast korzystania z danych stałe przeprowadzili zapytanie do bazy danych w celu pobrania informacji.

1. Otwórz **Controllers\StoreController.cs** i Dodaj następujące pole do klasy, aby pomieścić wystąpienie klasy **MusicStoreEntities** o nazwie **storeDB**:

    (Fragment kodu — *modele i dostęp do danych — Ex1 storeDB*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
2. Klasa **MusicStoreEntities** uwidacznia Właściwość kolekcji dla każdej tabeli w bazie danych. Aktualizowanie metody akcji **przeglądania** w celu pobrania gatunku z wszystkimi **albumami**.

    (Fragment kodu — *modele i dostęp do danych — Ex1 Store Browse*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

    > [!NOTE]
    > Używasz możliwości platformy .NET o nazwie **LINQ** (załączonej do języka) do pisania wyrażeń zapytania o jednoznacznie określonym typie względem tych kolekcji, które będą wykonywać kod względem bazy danych i zwracać obiekty, dla których można programować.
    > 
    > Więcej informacji na temat programu LINQ można znaleźć w [witrynie MSDN](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).
3. Zaktualizuj metodę akcji **indeksu** w celu pobrania wszystkich gatunków.

    (Fragment kodu — *modele i dostęp do danych — indeks magazynu Ex1*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
4. Zaktualizuj metodę akcji **indeksu** w celu pobrania wszystkich gatunków i przekształcenia kolekcji na listę.

    (Fragment kodu — *modele i dostęp do danych — Ex1 Store GenreMenu*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Zadanie 5 — Uruchamianie aplikacji

W tym zadaniu sprawdzisz, że na stronie indeks sklepu będą teraz wyświetlane gatunki przechowywane w bazie danych zamiast stałe. Nie ma potrzeby zmiany szablonu widoku, ponieważ **StoreController** zwraca te same jednostki co poprzednio, chociaż ten czas będzie pochodzący z bazy danych.

1. Skompiluj ponownie rozwiązanie i naciśnij klawisz **F5** , aby uruchomić aplikację.
2. Projekt zostanie uruchomiony na stronie głównej. Sprawdź, czy menu **gatunek** nie jest już listą stałe, a dane są bezpośrednio pobierane z bazy danych.

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    *Przeglądanie gatunków z bazy danych*
3. Teraz przejdź do dowolnego gatunku i sprawdź, czy albumy są wypełnione z bazy danych.

    ![Przeglądanie albumów z bazy danych](aspnet-mvc-4-models-and-data-access/_static/image17.png "Przeglądanie albumów z bazy danych")

    *Przeglądanie albumów z bazy danych*

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a>Ćwiczenie 2: Tworzenie bazy danych przy użyciu Code First

W tym ćwiczeniu dowiesz się, jak za pomocą podejścia Code First utworzyć bazę danych z tabelami aplikacji MusicStore oraz jak uzyskać dostęp do swoich danych.

Po wygenerowaniu modelu zmodyfikujesz StoreController, aby udostępnić szablon widoku z danymi pobranymi z bazy danych, zamiast używać wartości stałe.

> [!NOTE]
> Jeśli wykonano 1 ćwiczenie i już pracowałeś z podejściem Database First, dowiesz się, jak uzyskać te same wyniki przy użyciu innego procesu. Zadania, które są wspólne w ćwiczeniu 1, zostały oznaczone, aby ułatwić odczytywanie. Jeśli nie zakończysz ćwiczenia 1, ale chcesz poznać podejście Code First, możesz zacząć od tego ćwiczenia i uzyskać pełen zakres tematu.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a>Zadanie 1 — zapełnianie przykładowych danych

W tym zadaniu zostanie wypełniona baza danych z przykładowymi danymi, gdy zostanie ona początkowo utworzona przy użyciu kodu.

1. Otwórz rozwiązanie **BEGIN** znajdujące się w lokalizacji **Source/Ex2-CreatingADatabaseCodeFirst/BEGIN/** folder. W przeciwnym razie można nadal korzystać z rozwiązania **końcowego** uzyskanego przez zakończenie poprzedniego ćwiczenia.

   1. Jeśli otwarto **udostępnione rozwiązanie** , należy pobrać brakujące pakiety NuGet przed kontynuowaniem. W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.
   2. W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu. Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu. To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.
2. Dodaj plik **SampleData.cs** do folderu **models** . W tym celu kliknij prawym przyciskiem myszy folder **modele** , wskaż polecenie **Dodaj** , a następnie kliknij pozycję **istniejący element**. Przejdź do **\Source\Assets** i wybierz plik **SampleData.cs** .

    ![Przykładowy kod uzupełniający dane](aspnet-mvc-4-models-and-data-access/_static/image18.png "Przykładowy kod uzupełniający dane")

    *Przykładowy kod uzupełniający dane*
3. Otwórz plik **Global.asax.cs** i Dodaj następujące instrukcje *using* .

    (Fragment kodu — *modele i dostęp do danych — Ex2 Global asax using*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
4. Aby ustawić inicjatora bazy danych, w metodzie **Start\_aplikacji** Dodaj następujący wiersz.

    (Fragment kodu — *modele i dostęp do danych — Ex2 Global asax Setinitializeer*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a>Zadanie 2 — Konfigurowanie połączenia z bazą danych

Teraz, gdy baza danych została już dodana do projektu, należy napisać w pliku **Web. config** parametry połączenia.

1. Dodaj parametry połączenia w **pliku Web. config**. W tym celu Otwórz **plik Web. config** w katalogu głównym projektu i Zastąp ciąg połączenia o nazwie DefaultConnection tym wierszem w sekcji **&lt;connectionStrings&gt;** :

    ![Lokalizacja pliku Web. config](aspnet-mvc-4-models-and-data-access/_static/image19.png "Lokalizacja pliku Web. config")

    *Lokalizacja pliku Web. config*

    [!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a>Zadanie 3 — Praca z modelem

Teraz, gdy połączenie z bazą danych zostało już skonfigurowane, możesz połączyć model z tabelami baz danych. W tym zadaniu utworzysz klasę, która będzie połączona z bazą danych Code First. Należy pamiętać, że istnieje Klasa modelu POCO, która powinna zostać zmodyfikowana.

> [!NOTE]
> Jeśli wykonasz ćwiczenia 1, ten krok został wykonany przez kreatora. Wykonując Code First, ręcznie utworzysz klasy, które zostaną połączone z jednostkami danych.

1. Otwórz **gatunek** klasy modelu poco z folderu **models** Project i Dołącz identyfikator. Użyj właściwości int o nazwie **GenreId**.

    (Fragment kodu — *modele i dostęp do danych — Ex2 Code First gatunek*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

    > [!NOTE]
    > Aby można było korzystać z Konwencji Code First, gatunek klasy musi mieć właściwość klucza podstawowego, która zostanie automatycznie wykryta.
    > 
    > Więcej informacji na temat Konwencji Code First można znaleźć w tym [artykule w witrynie MSDN](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).
2. Teraz Otwórz **album** klasy modelu poco z folderu **models** Project i Dołącz klucze obce, Utwórz właściwości o nazwach **GenreId** i **ArtistId**. Ta klasa ma już **GenreId** klucz podstawowy.

    (Fragment kodu — *modele i dostęp do danych — Ex2 Code First albumu*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
3. Otwórz **wykonawcę** klasy modelu POCO i Uwzględnij Właściwość **ArtistId** .

    (Fragment kodu — *modele i dostęp do danych — Ex2 Code First wykonawcze*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
4. Kliknij prawym przyciskiem myszy folder **modele** projektu i wybierz polecenie **Dodaj | Klasa**. Nazwij plik **MusicStoreEntities.cs**. Następnie kliknij przycisk **Dodaj.**

    ![Dodawanie klasy](aspnet-mvc-4-models-and-data-access/_static/image20.png "Dodawanie klasy")

    *Dodawanie nowego elementu*

    ![Dodawanie elementu 'klasa](aspnet-mvc-4-models-and-data-access/_static/image21.png "Dodawanie elementu 'klasa")

    *Dodawanie klasy*
5. Otwórz właśnie utworzoną klasę, **MusicStoreEntities.cs**i Uwzględnij przestrzeń nazw **System. Data. Entity** i **System. Data. Entity. Infrastructure**.

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
6. Zastąp deklarację klasy, aby zwiększyć klasę **DbContext** : Zadeklaruj publiczną **nieogólnymi** i Zastąp metodę **OnModelCreating** . Po wykonaniu tego kroku otrzymasz klasę domeny, która będzie łączyć model z Entity Framework. Aby to zrobić, Zastąp kod klasy następującym:

    (Fragment kodu — *modele i dostęp do danych — Ex2 Code First MusicStoreEntities*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> Dzięki Entity Framework **DbContext** i **nieogólnymi** będzie można badać gatunek klasy poco. Rozszerzając metodę **OnModelCreating** , określasz w **kodzie** , jak gatunek zostanie zamapowany do tabeli bazy danych. Więcej informacji na temat DbContext i Nieogólnymi można znaleźć w tym artykule MSDN: [link](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>Zadanie 4 — wykonywanie zapytania dotyczącego bazy danych

W tym zadaniu zostanie zaktualizowana Klasa StoreController, tak aby zamiast korzystania z danych stałe była pobierana z bazy danych.

> [!NOTE]
> To zadanie jest wspólne w ćwiczeniu 1.
> 
> Jeśli wykonasz Ćwiczenie 1, te kroki są takie same w obu podejściach (najpierw baza danych lub kod w pierwszej kolejności). Różnią się one sposobem, w jaki dane są połączone z modelem, ale dostęp do jednostek danych jest jeszcze niewidoczny dla kontrolera.

1. Otwórz **Controllers\StoreController.cs** i Dodaj następujące pole do klasy, aby pomieścić wystąpienie klasy **MusicStoreEntities** o nazwie **storeDB**:

    (Fragment kodu — *modele i dostęp do danych — Ex1 storeDB*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
2. Klasa **MusicStoreEntities** uwidacznia Właściwość kolekcji dla każdej tabeli w bazie danych. Aktualizowanie metody akcji **przeglądania** w celu pobrania gatunku z wszystkimi **albumami**.

    (Fragment kodu — *modele i dostęp do danych — Ex2 Store Browse*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

    > [!NOTE]
    > Używasz możliwości platformy .NET o nazwie **LINQ** (załączonej do języka) do pisania wyrażeń zapytania o jednoznacznie określonym typie względem tych kolekcji, które będą wykonywać kod względem bazy danych i zwracać obiekty, dla których można programować.
    > 
    > Więcej informacji na temat programu LINQ można znaleźć w [witrynie MSDN](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).
3. Zaktualizuj metodę akcji **indeksu** w celu pobrania wszystkich gatunków.

    (Fragment kodu — *modele i dostęp do danych — indeks magazynu Ex2*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
4. Zaktualizuj metodę akcji **indeksu** w celu pobrania wszystkich gatunków i przekształcenia kolekcji na listę.

    (Fragment kodu — *modele i dostęp do danych — Ex2 Store GenreMenu*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Zadanie 5 — Uruchamianie aplikacji

W tym zadaniu sprawdzisz, że na stronie indeks sklepu będą teraz wyświetlane gatunki przechowywane w bazie danych zamiast stałe. Nie ma potrzeby zmiany szablonu widoku, ponieważ **StoreController** zwraca taką samą **StoreIndexViewModel** jak poprzednio, ale ten czas danych pochodzi z bazy danych.

1. Skompiluj ponownie rozwiązanie i naciśnij klawisz **F5** , aby uruchomić aplikację.
2. Projekt zostanie uruchomiony na stronie głównej. Sprawdź, czy menu **gatunek** nie jest już listą stałe, a dane są bezpośrednio pobierane z bazy danych.

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    *Przeglądanie gatunków z bazy danych*
3. Teraz przejdź do dowolnego gatunku i sprawdź, czy albumy są wypełnione z bazy danych.

    ![Przeglądanie albumów z bazy danych](aspnet-mvc-4-models-and-data-access/_static/image23.png "Przeglądanie albumów z bazy danych")

    *Przeglądanie albumów z bazy danych*

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a>Ćwiczenie 3: wykonywanie zapytania dotyczącego bazy danych za pomocą parametrów

W tym ćwiczeniu dowiesz się, jak wysyłać zapytania do bazy danych przy użyciu parametrów i jak używać kształtowania wyników zapytania, a funkcja, która zmniejsza liczbę baz danych, uzyskuje dostęp do danych w bardziej wydajny sposób.

> [!NOTE]
> Więcej informacji o kształtach wyników zapytania można znaleźć w następującym [artykule w witrynie MSDN](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a>Zadanie 1 — modyfikowanie StoreController w celu pobrania albumów z bazy danych

W tym zadaniu zmienimy klasę **StoreController** , aby uzyskać dostęp do bazy danych w celu pobrania albumów z określonego gatunku.

1. Otwórz rozwiązanie **BEGIN** znajdujące się w folderze **Source\Ex3-QueryingTheDatabaseWithParametersCodeFirst\Begin** , jeśli chcesz użyć podejścia najpierw do kodu lub folderu **Source\Ex3-QueryingTheDatabaseWithParametersDBFirst\Begin** , jeśli chcesz użyć podejścia pierwszej bazy danych. W przeciwnym razie można nadal korzystać z rozwiązania **końcowego** uzyskanego przez zakończenie poprzedniego ćwiczenia.

   1. Jeśli otwarto **udostępnione rozwiązanie** , należy pobrać brakujące pakiety NuGet przed kontynuowaniem. W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.
   2. W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu. Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu. To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.
2. Otwórz klasę **StoreController** , aby zmienić metodę **przeglądania** akcji. W tym celu w **Eksplorator rozwiązań**rozwiń folder **Kontrolery** i kliknij dwukrotnie pozycję **StoreController.cs**.
3. Zmień metodę akcji **Przeglądaj** , aby pobrać albumy dla określonego gatunku. Aby to zrobić, Zastąp następujący kod:

    (Fragment kodu — *modele i dostęp do danych — Ex3 StoreController BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> Aby wypełnić kolekcję jednostki, musisz użyć metody **include** , aby określić, że chcesz pobrać albumy. Możesz użyć. Rozszerzenie **Single ()** w LINQ, ponieważ w tym przypadku oczekiwany jest tylko jeden gatunek dla albumu. **Pojedyncza Metoda ()** przyjmuje wyrażenie lambda jako parametr, który w tym przypadku określa pojedynczy obiekt gatunku, tak że jego nazwa pasuje do zdefiniowanej wartości.
> 
> Będziesz korzystać z funkcji, która pozwala wskazać inne powiązane jednostki, które mają być załadowane, a także po pobraniu obiektu gatunku. Ta funkcja jest nazywana **kształtem wyników zapytania**i umożliwia zredukowanie liczby prób uzyskania dostępu do bazy danych w celu pobrania informacji. W tym scenariuszu warto wstępnie pobrać albumy dla danego gatunku.
> 
> Zapytanie zawiera **gatunek. Dołącz (&quot;albumy&quot;)** , aby wskazać, że chcesz również pokrewnych albumów. Spowoduje to wydajniejszą aplikację, ponieważ spowoduje pobieranie zarówno danych o gatunku, jak i albumie w ramach pojedynczego żądania bazy danych.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Zadanie 2 — Uruchamianie aplikacji

W tym zadaniu uruchomisz aplikację i pobierzesz albumy określonego gatunku z bazy danych.

1. Naciśnij klawisz **F5** , aby uruchomić aplikację.
2. Projekt zostanie uruchomiony na stronie głównej. Zmień adres URL na **/Store/Browse? gatunek = pop** , aby sprawdzić, czy wyniki są pobierane z bazy danych.

    ![Przeglądanie według gatunku](aspnet-mvc-4-models-and-data-access/_static/image24.png "Przeglądanie według gatunku")

    *Przeglądanie/Store/Browse? gatunek = pop*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a>Zadanie 3 — dostęp do albumów według identyfikatora

W tym zadaniu ponowimy poprzednią procedurę, aby uzyskać albumy według ich identyfikatorów.

1. W razie potrzeby Zamknij przeglądarkę, aby powrócić do programu Visual Studio. Otwórz klasę **StoreController** , aby zmienić metodę akcji **Details** . W tym celu w **Eksplorator rozwiązań**rozwiń folder **Kontrolery** i kliknij dwukrotnie pozycję **StoreController.cs**.
2. Zmień metodę akcji **Details** , aby pobrać szczegóły dotyczące albumów w oparciu o ich **identyfikatory**. Aby to zrobić, Zastąp następujący kod:

    (Fragment kodu — *modele i dostęp do danych — Ex3 StoreController DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Zadanie 4 — Uruchamianie aplikacji

W tym zadaniu aplikacja zostanie uruchomiona w przeglądarce internetowej i uzyska szczegóły dotyczące albumu według ich identyfikatora.

1. Naciśnij klawisz **F5** , aby uruchomić aplikację.
2. Projekt zostanie uruchomiony na stronie głównej. Zmień adres URL na **/Store/Details/51** lub Przeglądaj gatunek i wybierz album, aby sprawdzić, czy wyniki są pobierane z bazy danych.

    ![Szczegóły przeglądania](aspnet-mvc-4-models-and-data-access/_static/image25.png "Szczegóły przeglądania")

    *Przeglądanie/Store/Details/51*

> [!NOTE]
> Ponadto tę aplikację można wdrożyć w witrynach sieci Web systemu Windows Azure, które zostały opisane w [dodatku B: publikowanie aplikacji ASP.NET MVC 4 przy użyciu Web Deploy](#AppendixB).

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

Wypełniając te praktyczne laboratorium, uczysz się podstaw modeli ASP.NET MVC i dostępu do danych, korzystając z podejścia **Database First** , a także podejścia **Code Firstowego** :

- Jak dodać bazę danych do rozwiązania w celu korzystania z jego danych
- Jak zaktualizować kontrolery, aby udostępnić szablony widoków z danymi pobranymi z bazy danych zamiast na stałe.
- Jak wysyłać zapytania do bazy danych przy użyciu parametrów
- Jak używać kształtu wyniku zapytania, funkcji, która zmniejsza liczbę dostępu do bazy danych, pobierając dane w bardziej wydajny sposób
- Jak używać podejścia Database First i Code First w programie Microsoft Entity Framework do łączenia bazy danych z modelem

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Dodatek A: Instalowanie Visual Studio Express 2012 dla sieci Web

Można zainstalować **Microsoft Visual Studio Express 2012 dla sieci Web** lub innej &quot;Express&quot; wersji za pomocą **[Instalator platformy Microsoft Web](https://www.microsoft.com/web/downloads/platform.aspx)** . Poniższe instrukcje przeprowadzą Cię przez kroki wymagane do zainstalowania *programu Visual Studio Express 2012 for Web* przy użyciu *Instalator platformy Microsoft Web*.

1. Przejdź do [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Alternatywnie, jeśli zainstalowano już Instalatora platformy sieci Web, można go otworzyć i wyszukać produkt &quot;<em>Visual Studio Express 2012 for Web przy użyciu zestawu Windows Azure SDK</em>&quot;.
2. Kliknij pozycję **Zainstaluj teraz**. Jeśli nie masz **Instalatora platformy sieci Web** , nastąpi przekierowanie do pobrania i zainstalowania go w pierwszej kolejności.
3. Po otwarciu **Instalatora platformy sieci Web** kliknij przycisk **Zainstaluj** , aby rozpocząć instalację.

    ![Zainstaluj Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "Zainstaluj Visual Studio Express")

    *Zainstaluj Visual Studio Express*
4. Przeczytaj wszystkie licencje i postanowienia produktów, a następnie kliknij przycisk **Akceptuję** , aby kontynuować.

    ![Akceptowanie postanowień licencyjnych](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    *Akceptowanie postanowień licencyjnych*
5. Poczekaj na zakończenie procesu pobierania i instalacji.

    ![Postęp instalacji](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    *Postęp instalacji*
6. Po zakończeniu instalacji kliknij przycisk **Zakończ**.

    ![Instalacja zakończona](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    *Instalacja zakończona*
7. Kliknij przycisk **Zakończ** , aby zamknąć Instalatora platformy sieci Web.
8. Aby otworzyć Visual Studio Express dla sieci Web, przejdź do ekranu **startowego** i zacznij pisać &quot;**vs Express**&quot;, a następnie kliknij kafelek **vs Express for Web** .

    ![Kafelek VS Express for Web](aspnet-mvc-4-models-and-data-access/_static/image30.png)

    *Kafelek VS Express for Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Dodatek B: publikowanie aplikacji ASP.NET MVC 4 przy użyciu Web Deploy

W tym dodatku pokazano, jak utworzyć nową witrynę sieci Web na podstawie portal zarządzania systemu Windows Azure i opublikować aplikację uzyskaną w wyniku działania laboratorium, korzystając z funkcji publikowania Web Deploy dostępnej w systemie Windows Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Zadanie 1 — Tworzenie nowej witryny sieci Web z poziomu portalu systemu Windows Azure

1. Przejdź do [Portal zarządzania systemu Windows Azure](https://manage.windowsazure.com/) i zaloguj się przy użyciu poświadczeń firmy Microsoft skojarzonych z Twoją subskrypcją.

    > [!NOTE]
    > System Windows Azure umożliwia bezpłatne hostowanie witryn sieci Web 10 ASP.NET, a następnie skalowanie ich w miarę wzrostu ruchu. Możesz utworzyć konto w [tym miejscu](https://aka.ms/aspnet-hol-azure).

    ![Zaloguj się do systemu Windows Azure Portal](aspnet-mvc-4-models-and-data-access/_static/image31.png "Zaloguj się do systemu Windows Azure Portal")

    *Zaloguj się do usługi Windows Azure portal zarządzania*
2. Na pasku poleceń kliknij pozycję **Nowy** .

    ![Tworzenie nowej witryny sieci Web](aspnet-mvc-4-models-and-data-access/_static/image32.png "Tworzenie nowej witryny sieci Web")

    *Tworzenie nowej witryny sieci Web*
3. Kliknij pozycję **obliczeniowe** | **witrynie sieci Web**. Następnie wybierz opcję **szybkie tworzenie** . Podaj dostępny adres URL dla nowej witryny sieci Web i kliknij pozycję **Utwórz witrynę sieci Web**.

    > [!NOTE]
    > Witryna sieci Web systemu Windows Azure jest hostem aplikacji sieci Web działającej w chmurze, którą można kontrolować i zarządzać nią. Opcja szybkie tworzenie umożliwia wdrożenie ukończonej aplikacji sieci Web w witrynie sieci Web systemu Windows Azure spoza portalu. Nie obejmuje to kroków związanych z konfigurowaniem bazy danych.

    ![Tworzenie nowej witryny sieci Web przy użyciu funkcji szybkiego tworzenia](aspnet-mvc-4-models-and-data-access/_static/image33.png "Tworzenie nowej witryny sieci Web przy użyciu funkcji szybkiego tworzenia")

    *Tworzenie nowej witryny sieci Web przy użyciu funkcji szybkiego tworzenia*
4. Poczekaj na utworzenie nowej **witryny sieci Web** .
5. Po utworzeniu witryny sieci Web kliknij link pod kolumną **adres URL** . Sprawdź, czy nowa witryna sieci Web działa.

    ![Przeglądanie w nowej witrynie sieci Web](aspnet-mvc-4-models-and-data-access/_static/image34.png "Przeglądanie w nowej witrynie sieci Web")

    *Przeglądanie w nowej witrynie sieci Web*

    ![Uruchomiona witryna sieci Web](aspnet-mvc-4-models-and-data-access/_static/image35.png "Uruchomiona witryna sieci Web")

    *Uruchomiona witryna sieci Web*
6. Wróć do portalu i kliknij nazwę witryny sieci Web pod kolumną **Nazwa** , aby wyświetlić strony zarządzania.

    ![Otwieranie stron zarządzania witryną sieci Web](aspnet-mvc-4-models-and-data-access/_static/image36.png "Otwieranie stron zarządzania witryną sieci Web")

    *Otwieranie stron zarządzania witryną sieci Web*
7. Na stronie **pulpit nawigacyjny** w sekcji **szybki przegląd** kliknij link **Pobierz profil publikowania** .

    > [!NOTE]
    > *Profil publikowania* zawiera wszystkie informacje wymagane do opublikowania aplikacji sieci Web w witrynie sieci Web systemu Windows Azure dla każdej z włączonych metod publikacji. Profil publikowania zawiera adresy URL, poświadczenia użytkownika i ciągi bazy danych wymagane do nawiązania połączenia i uwierzytelniania dla każdego punktu końcowego, dla którego jest włączona metoda publikacji. **Program Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** i **Microsoft Visual Studio 2012** obsługują odczytywanie profilów publikowania w celu zautomatyzowania konfiguracji tych programów do publikowania aplikacji sieci Web w usłudze Microsoft Azure Websites.

    ![Pobieranie profilu publikowania witryny sieci Web](aspnet-mvc-4-models-and-data-access/_static/image37.png "Pobieranie profilu publikowania witryny sieci Web")

    *Pobieranie profilu publikowania witryny sieci Web*
8. Pobierz plik profilu publikowania do znanej lokalizacji. W tym ćwiczeniu zobaczysz, jak używać tego pliku do publikowania aplikacji sieci Web w witrynach sieci Web systemu Windows Azure z programu Visual Studio.

    ![Zapisywanie pliku profilu publikowania](aspnet-mvc-4-models-and-data-access/_static/image38.png "Zapisywanie profilu publikowania")

    *Zapisywanie pliku profilu publikowania*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Zadanie 2 — Konfigurowanie serwera bazy danych

Jeśli aplikacja korzysta z baz danych SQL Server, należy utworzyć SQL Database serwer. Jeśli chcesz wdrożyć prostą aplikację, która nie używa SQL Server można pominąć to zadanie.

1. Do przechowywania bazy danych aplikacji potrzebny jest serwer SQL Database. Serwery SQL Database z subskrypcji można wyświetlić w portalu zarządzania systemu Windows Azure w obszarze **bazy danych SQL** | **serwery** | **pulpitu nawigacyjnego serwera**. Jeśli nie masz utworzonego serwera, możesz go utworzyć przy użyciu przycisku **Dodaj** na pasku poleceń. Zanotuj **nazwę serwera i adres URL, nazwę logowania administratora i hasło**, ponieważ zostaną one użyte w następnych zadaniach. Nie Twórz jeszcze bazy danych, ponieważ zostanie ona utworzona na późniejszym etapie.

    ![Pulpit nawigacyjny serwera SQL Database](aspnet-mvc-4-models-and-data-access/_static/image39.png "Pulpit nawigacyjny serwera SQL Database")

    *Pulpit nawigacyjny serwera SQL Database*
2. W następnym zadaniu zostanie przetestowane połączenie z bazą danych z programu Visual Studio. z tego powodu należy uwzględnić lokalny adres IP na liście **dozwolonych adresów IP**serwera. W tym celu kliknij pozycję **Konfiguruj**, wybierz adres IP z **bieżącego adresu IP klienta** i wklej go w polach tekstowych **początkowy adres IP** i **końcowy adres IP** , a następnie kliknij przycisk ![Dodaj-Client-IP-Address-OK-Button](aspnet-mvc-4-models-and-data-access/_static/image40.png).

    ![Dodawanie adresu IP klienta](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    *Dodawanie adresu IP klienta*
3. Po dodaniu **adresu IP klienta** do listy dozwolone adresy IP kliknij pozycję **Zapisz** , aby potwierdzić zmiany.

    ![Potwierdź zmiany](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    *Potwierdź zmiany*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Zadanie 3 — publikowanie aplikacji ASP.NET MVC 4 przy użyciu Web Deploy

1. Wróć do rozwiązania ASP.NET MVC 4. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt witryny sieci Web i wybierz polecenie **Publikuj**.

    ![Publikowanie aplikacji](aspnet-mvc-4-models-and-data-access/_static/image43.png "Publikowanie aplikacji")

    *Publikowanie witryny sieci Web*
2. Zaimportuj profil publikowania zapisany w pierwszym zadaniu.

    ![Importowanie profilu publikowania](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importowanie profilu publikowania")

    *Importowanie profilu publikowania*
3. Kliknij pozycję **Weryfikuj połączenie**. Po zakończeniu walidacji kliknij przycisk **dalej**.

    > [!NOTE]
    > Walidacja jest ukończona po wyświetleniu zielonego znacznika wyboru obok przycisku Weryfikuj połączenie.

    ![Weryfikowanie połączenia](aspnet-mvc-4-models-and-data-access/_static/image45.png "Weryfikowanie połączenia")

    *Weryfikowanie połączenia*
4. Na stronie **Ustawienia** w sekcji **bazy danych** kliknij przycisk obok pola tekstowego połączenie z bazą danych (np. **DefaultConnection**).

    ![Konfiguracja narzędzia Web Deploy](aspnet-mvc-4-models-and-data-access/_static/image46.png "Konfiguracja narzędzia Web Deploy")

    *Konfiguracja narzędzia Web Deploy*
5. Skonfiguruj połączenie z bazą danych w następujący sposób:

   - W polu **Nazwa serwera** wpisz adres URL serwera SQL Database przy użyciu prefiksu *TCP:* .
   - W polu **Nazwa użytkownika** wpisz nazwę logowania administratora serwera.
   - W polu **hasło** wpisz hasło logowania administratora serwera.
   - Wpisz nową nazwę bazy danych.

     ![Konfigurowanie parametrów połączenia docelowego](aspnet-mvc-4-models-and-data-access/_static/image47.png "Konfigurowanie parametrów połączenia docelowego")

     *Konfigurowanie parametrów połączenia docelowego*
6. Następnie kliknij przycisk **OK**. Po wyświetleniu monitu o utworzenie bazy danych kliknij przycisk **tak**.

    ![Tworzenie bazy danych](aspnet-mvc-4-models-and-data-access/_static/image48.png "Tworzenie ciągu bazy danych")

    *Tworzenie bazy danych*
7. Parametry połączenia, które będą używane do nawiązywania połączenia z SQL Database w systemie Windows Azure, są wyświetlane w domyślnym polu tekstowym połączenie. Następnie kliknij przycisk **Next** (Dalej).

    ![Parametry połączenia wskazujące SQL Database](aspnet-mvc-4-models-and-data-access/_static/image49.png "Parametry połączenia wskazujące SQL Database")

    *Parametry połączenia wskazujące SQL Database*
8. Na stronie **Podgląd** kliknij przycisk **Publikuj**.

    ![Publikowanie aplikacji sieci Web](aspnet-mvc-4-models-and-data-access/_static/image50.png "Publikowanie aplikacji sieci Web")

    *Publikowanie aplikacji sieci Web*
9. Po zakończeniu procesu publikowania w domyślnej przeglądarce zostanie otwarta opublikowana witryna sieci Web.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Dodatek C: używanie fragmentów kodu

Za pomocą fragmentów kodu masz cały kod, którego potrzebujesz na wyręką. Dokument laboratoryjny powiedzie się dokładnie wtedy, gdy można z nich korzystać, jak pokazano na poniższej ilustracji.

![Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio](aspnet-mvc-4-models-and-data-access/_static/image51.png "Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio")

*Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio*

***Aby dodać fragment kodu przy użyciu klawiatury (C# tylko)***

1. Umieść kursor, w którym chcesz wstawić kod.
2. Zacznij wpisywać nazwę fragmentu kodu (bez spacji lub łączników).
3. Obejrzyj jako IntelliSense wyświetla pasujące nazwy fragmentów kodu.
4. Wybierz poprawny fragment kodu (lub Kontynuuj wpisywanie, dopóki nie zostanie zaznaczona cała nazwa fragmentu kodu).
5. Naciśnij dwukrotnie klawisz Tab, aby wstawić fragment kodu w lokalizacji kursora.

![Zacznij wpisywać nazwę fragmentu kodu](aspnet-mvc-4-models-and-data-access/_static/image52.png "Zacznij wpisywać nazwę fragmentu kodu")

*Zacznij wpisywać nazwę fragmentu kodu*

![Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu](aspnet-mvc-4-models-and-data-access/_static/image53.png "Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu")

*Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu*

![Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta](aspnet-mvc-4-models-and-data-access/_static/image54.png "Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta")

*Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta*

***Aby dodać fragment kodu przy użyciu myszy (C#, Visual Basic i XML)*** jedno. Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu.

1. Wybierz **Wstaw fragment** kodu, po którym następuje **Moje fragmenty kodów**.
2. Wybierz odpowiedni fragment kodu z listy, klikając go.

![Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment](aspnet-mvc-4-models-and-data-access/_static/image55.png "Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment")

*Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment*

![Wybierz odpowiedni fragment kodu z listy, klikając go](aspnet-mvc-4-models-and-data-access/_static/image56.png "Wybierz odpowiedni fragment kodu z listy, klikając go")

*Wybierz odpowiedni fragment kodu z listy, klikając go*

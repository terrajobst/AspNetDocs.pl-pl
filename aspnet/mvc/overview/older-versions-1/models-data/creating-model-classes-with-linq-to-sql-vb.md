---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-vb
title: Tworzenie klas modelu za pomocą LINQ to SQL (VB) | Microsoft Docs
author: microsoft
description: Celem tego samouczka jest wyjaśnienie jednej metody tworzenia klas modelu dla aplikacji ASP.NET MVC. W tym samouczku dowiesz się, jak skompilować model c...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: a4a25a75-d71f-4509-98b4-df72e748985a
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-vb
msc.type: authoredcontent
ms.openlocfilehash: 88a5f1037d93ef3bdc95bf60b6005ebb254ab440
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74588578"
---
# <a name="creating-model-classes-with-linq-to-sql-vb"></a>Tworzenie klas modeli za pomocą modelu LINQ to SQL (VB)

przez [firmę Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](https://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_10_VB.pdf)

> Celem tego samouczka jest wyjaśnienie jednej metody tworzenia klas modelu dla aplikacji ASP.NET MVC. W ramach tego samouczka nauczysz się tworzyć klasy modelu i uzyskiwać dostęp do bazy danych, korzystając z programu Microsoft LINQ to SQL.

Celem tego samouczka jest wyjaśnienie jednej metody tworzenia klas modelu dla aplikacji ASP.NET MVC. W ramach tego samouczka nauczysz się tworzyć klasy modelu i uzyskiwać dostęp do bazy danych, korzystając z programu Microsoft LINQ to SQL.

W tym samouczku tworzymy podstawową aplikację bazy danych filmu. Zaczynamy od utworzenia aplikacji bazy danych filmów w najszybszy i najprostszy sposób. Cały dostęp do danych jest realizowany bezpośrednio z naszych akcji kontrolera.

Następnie dowiesz się, jak używać wzorca repozytorium. Użycie wzorca repozytorium wymaga nieco większego nakładu pracy. Jednak zaletą wdrożenia tego wzorca jest umożliwienie tworzenia aplikacji, które można dostosować do zmian i mogą być łatwe do przetestowania.

## <a name="what-is-a-model-class"></a>Co to jest Klasa modelu?

Model MVC zawiera całą logikę aplikacji, która nie jest zawarta w widoku MVC lub kontrolera MVC. W szczególności model MVC zawiera całą logikę dostępu do aplikacji i danych firmy.

Aby zaimplementować logikę dostępu do danych, można użyć różnych technologii. Na przykład można skompilować klasy dostępu do danych przy użyciu klas Microsoft Entity Framework, NHibernate, Subsonic lub ADO.NET.

W tym samouczku użyjemy LINQ to SQL, aby wysyłać zapytania i aktualizować bazę danych. LINQ to SQL zapewnia bardzo łatwą metodę korzystania z Microsoft SQL Server Database. Należy jednak pamiętać, że struktura ASP.NET MVC nie jest powiązana z LINQ to SQL w jakikolwiek sposób. ASP.NET MVC jest zgodny z dowolnymi technologiami dostępu do danych.

## <a name="create-a-movie-database"></a>Tworzenie bazy danych filmów

W tym samouczku — w celu zilustrowania sposobu tworzenia klas modelu — tworzymy prostą aplikację bazy danych filmów. Pierwszym krokiem jest utworzenie nowej bazy danych. Kliknij prawym przyciskiem myszy folder\_danych aplikacji w oknie Eksplorator rozwiązań i wybierz opcję menu **Dodaj, nowy element**. Wybierz szablon bazy danych SQL Server, nadaj mu nazwę MoviesDB. mdf, a następnie kliknij przycisk **Dodaj** (patrz rysunek 1).

[![Dodawanie nowej bazy danych SQL Server](creating-model-classes-with-linq-to-sql-vb/_static/image2.png)](creating-model-classes-with-linq-to-sql-vb/_static/image1.png)

**Ilustracja 01**. Dodawanie nowej bazy danych SQL Server ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-model-classes-with-linq-to-sql-vb/_static/image3.png))

Po utworzeniu nowej bazy danych można otworzyć ją, klikając dwukrotnie plik MoviesDB. mdf w folderze danych\_aplikacji. Dwukrotne kliknięcie pliku MoviesDB. mdf spowoduje otwarcie okna Eksplorator serwera (patrz rysunek 2).

|   | Okno Eksplorator serwera jest nazywane oknem Eksplorator bazy danych, gdy jest używany program Visual Web Developer. |
|---|----------------------------------------------------------------------------------------------------|
|   |                                                                                                    |

[![przy użyciu okna Eksplorator serwera](creating-model-classes-with-linq-to-sql-vb/_static/image5.png)](creating-model-classes-with-linq-to-sql-vb/_static/image4.png)

**Ilustracja 02**. Korzystanie z okna Eksplorator serwera ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-model-classes-with-linq-to-sql-vb/_static/image6.png))

Musimy dodać jedną tabelę do naszej bazy danych, która reprezentuje nasze filmy. Kliknij prawym przyciskiem myszy folder tabele i wybierz opcję menu **Dodaj nową tabelę**. Wybranie tej opcji menu spowoduje otwarcie projektanta tabel (zobacz rysunek 3).

[![przy użyciu okna Eksplorator serwera](creating-model-classes-with-linq-to-sql-vb/_static/image8.png)](creating-model-classes-with-linq-to-sql-vb/_static/image7.png)

**Ilustracja 03**: Projektant tabel ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-model-classes-with-linq-to-sql-vb/_static/image9.png))

Musimy dodać następujące kolumny do naszej tabeli bazy danych:

| **Nazwa kolumny** | **Typ danych** | **Zezwalaj na wartości null** |
| --- | --- | --- |
| #C1 | int | Fałsz |
| Tytuł | Nvarchar (200) | Fałsz |
| General | nvarchar (50) | Fałsz |

Należy wykonać dwa specjalne rzeczy do kolumny ID. Najpierw należy oznaczyć kolumnę ID jako kolumnę klucza podstawowego, wybierając kolumnę w Projektancie tabel i klikając ikonę klucza. LINQ to SQL wymaga określenia kolumn klucza podstawowego podczas wykonywania operacji wstawiania lub aktualizacji w bazie danych.

Następnie należy oznaczyć kolumnę ID jako kolumnę tożsamości, przypisując wartość Yes do właściwości **is Identity** (zobacz rysunek 3). Kolumna tożsamości jest kolumną, do której przypisano nowy numer automatycznie za każdym razem, gdy dodasz nowy wiersz danych do tabeli.

Po wprowadzeniu tych zmian Zapisz tabelę o nazwie tblMovie. Tabelę można zapisać, klikając przycisk Zapisz.

## <a name="create-linq-to-sql-classes"></a>Utwórz klasy LINQ to SQL

Nasz model MVC będzie zawierać klasy LINQ to SQL, które reprezentują tabelę bazy danych tblMovie. Najprostszym sposobem tworzenia tych klas LINQ to SQL jest kliknięcie prawym przyciskiem myszy folderu models, wybranie **pozycji Dodaj, nowy element**, wybranie szablonu klasy LINQ to SQL, nadanie klasom nazwy Movie. dbml, a następnie kliknięcie przycisku **Dodaj** (zobacz rysunek 4).

[![tworzenia klas LINQ to SQL](creating-model-classes-with-linq-to-sql-vb/_static/image11.png)](creating-model-classes-with-linq-to-sql-vb/_static/image10.png)

**Ilustracja 04**: tworzenie klas LINQ to SQL ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-model-classes-with-linq-to-sql-vb/_static/image12.png))

Natychmiast po utworzeniu klas LINQ to SQL filmów zostanie wyświetlony Object Relational Designer. Możesz przeciągnąć tabele bazy danych z okna Eksplorator serwera na Object Relational Designer, aby utworzyć klasy LINQ to SQL, które reprezentują konkretne tabele bazy danych. Musimy dodać tabelę bazy danych tblMovie do Object Relational Designer (zobacz rysunek 4).

[![przy użyciu Object Relational Designer](creating-model-classes-with-linq-to-sql-vb/_static/image14.png)](creating-model-classes-with-linq-to-sql-vb/_static/image13.png)

**Ilustracja 05**. Używanie Object Relational Designer ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-model-classes-with-linq-to-sql-vb/_static/image15.png))

Domyślnie Object Relational Designer tworzy klasę o takiej samej nazwie jak tabela bazy danych przeciągnięta do projektanta. Nie chcemy jednak nazywać naszej klasy tblMovie. W związku z tym kliknij nazwę klasy w Projektancie i Zmień nazwę klasy na film.

Na koniec pamiętaj o kliknięciu przycisku **Zapisz** (obraz dyskietki) w celu zapisania klas LINQ to SQL. W przeciwnym razie klasy LINQ to SQL nie będą generowane przez Object Relational Designer.

## <a name="using-linq-to-sql-in-a-controller-action"></a>Używanie LINQ to SQL w akcji kontrolera

Teraz, gdy mamy nasze klasy LINQ to SQL, możemy używać tych klas do pobierania danych z bazy danych. W tej sekcji dowiesz się, jak używać klas LINQ to SQL bezpośrednio w ramach akcji kontrolera. W widoku MVC zostanie wyświetlona lista filmów z tabeli bazy danych tblMovies.

Najpierw należy zmodyfikować klasę HomeController. Tę klasę można znaleźć w folderze controllers swojej aplikacji. Zmodyfikuj klasę, aby wyglądała jak Klasa w liście 1.

**Lista 1 — `Controllers\HomeController.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample1.vb)]

Akcja index () na liście 1 używa klasy DataContext LINQ to SQL (MovieDataContext) do reprezentowania bazy danych MoviesDB. Klasa MoveDataContext została wygenerowana przez Object Relational Designer programu Visual Studio.

Zapytanie LINQ jest wykonywane względem elementu DataContext w celu pobrania wszystkich filmów z tabeli bazy danych tblMovies. Lista filmów jest przypisana do zmiennej lokalnej o nazwie filmy. Na koniec lista filmów jest przenoszona do widoku poprzez wyświetlenie danych.

W celu pokazania filmów należy dalej zmodyfikować widok indeksu. Widok indeks można znaleźć w folderze Views\Home\. Zaktualizuj widok indeksu, aby wyglądał jak widok w liście 2.

**Lista 2 — `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample2.aspx)]

Należy zauważyć, że zmodyfikowany widok indeksu zawiera &lt;% @ import Namespace%&gt; dyrektywy w górnej części widoku. Ta dyrektywa importuje przestrzeń nazw MvcApplication1. Ta przestrzeń nazw jest wymagana do pracy z klasami modelu, w szczególności z klasą filmów — w widoku.

Widok w liście 2 zawiera pętlę for each, która iteruje przez wszystkie elementy reprezentowane przez właściwość ViewData. model. Wartość właściwości title jest wyświetlana dla każdego filmu.

Zwróć uwagę, że wartość właściwości ViewData. model jest rzutowana na interfejs IEnumerable. Jest to konieczne w celu przepętlenia przez zawartość ViewData. model. Inną opcją jest utworzenie widoku o jednoznacznie określonym typie. Podczas tworzenia widoku z silną typem należy rzutować Właściwość ViewData. model na określony typ w klasie z kodem związanym z widokiem.

Jeśli aplikacja zostanie uruchomiona po zmodyfikowaniu klasy HomeController i widoku indeksu, uzyskasz pustą stronę. Uzyskasz pustą stronę, ponieważ w tabeli bazy danych tblMovies nie ma żadnych rekordów filmów.

Aby dodać rekordy do tabeli bazy danych tblMovies, kliknij prawym przyciskiem myszy tabelę bazy danych tblMovies w oknie Eksplorator serwera (Eksplorator bazy danych oknie w Visual Web Developer) i wybierz opcję menu **Pokaż dane tabeli**. Rekordy filmów można wstawiać przy użyciu wyświetlonej siatki (patrz rysunek 5).

[![Wstawianie filmów](creating-model-classes-with-linq-to-sql-vb/_static/image17.png)](creating-model-classes-with-linq-to-sql-vb/_static/image16.png)

**Ilustracja 06**. Wstawianie filmów ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-model-classes-with-linq-to-sql-vb/_static/image18.png))

Po dodaniu rekordów bazy danych do tabeli tblMovies, gdy aplikacja zostanie uruchomiona, zostanie wyświetlona strona na rysunku 7. Wszystkie rekordy bazy danych filmów są wyświetlane na liście punktowanej.

[![wyświetlania filmów z widokiem indeksu](creating-model-classes-with-linq-to-sql-vb/_static/image20.png)](creating-model-classes-with-linq-to-sql-vb/_static/image19.png)

**Ilustracja 07**. Wyświetlanie filmów przy użyciu widoku indeksu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-model-classes-with-linq-to-sql-vb/_static/image21.png))

## <a name="using-the-repository-pattern"></a>Używanie wzorca repozytorium

W poprzedniej sekcji użyto LINQ to SQL klas bezpośrednio w ramach akcji kontrolera. Klasa MovieDataContext została użyta bezpośrednio z akcji kontrolera indeks (). W przypadku prostej aplikacji nie ma nic nieprawidłowego działania. Jednak praca bezpośrednio z LINQ to SQL w klasie kontrolera tworzy problemy, gdy trzeba utworzyć bardziej złożoną aplikację.

Użycie LINQ to SQL w ramach klasy kontrolera utrudnia przełączanie technologii dostępu do danych w przyszłości. Można na przykład przełączyć się z programu Microsoft LINQ to SQL do korzystania z usługi Microsoft Entity Framework jako technologii dostępu do danych. W takim przypadku należy ponownie napisać każdy kontroler, który uzyskuje dostęp do bazy danych w aplikacji.

Użycie LINQ to SQL w ramach klasy kontrolera sprawia, że trudno jest skompilować testy jednostkowe dla aplikacji. Zwykle nie ma potrzeby korzystania z bazy danych podczas przeprowadzania testów jednostkowych. Chcesz użyć testów jednostkowych do testowania logiki aplikacji, a nie serwera bazy danych.

Aby można było utworzyć aplikację MVC, która jest bardziej dostosowywana do przyszłych zmian i która może być łatwiejsza do przetestowania, należy rozważyć użycie wzorca repozytorium. Korzystając ze wzorca repozytorium, należy utworzyć oddzielną klasę repozytorium, która zawiera całą logikę dostępu do bazy danych.

Podczas tworzenia klasy repozytorium utworzysz interfejs reprezentujący wszystkie metody używane przez klasę repozytorium. W ramach Twoich kontrolerów napiszesz kod do interfejsu zamiast repozytorium. Dzięki temu można zaimplementować repozytorium przy użyciu różnych technologii dostępu do danych w przyszłości.

Interfejs na liście 3 ma nazwę IMovieRepository i reprezentuje jedną metodę o nazwie ListAll ().

**Lista 3 — `Models\IMovieRepository.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample3.vb)]

Klasa repozytorium w liście 4 implementuje interfejs IMovieRepository. Należy zauważyć, że zawiera ona metodę o nazwie ListAll () odpowiadającą metodzie wymaganej przez interfejs IMovieRepository.

**Lista 4 — `Models\MovieRepository.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample4.vb)]

Na koniec Klasa MoviesController na liście 5 używa wzorca repozytorium. Nie używa już bezpośrednio klas LINQ to SQL.

**Lista 5 — `Controllers\MoviesController.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample5.vb)]

Należy zauważyć, że Klasa MoviesController na liście 5 ma dwa konstruktory. Pierwszy Konstruktor, Konstruktor bez parametrów jest wywoływany, gdy aplikacja jest uruchomiona. Ten konstruktor tworzy wystąpienie klasy MovieRepository i przekazuje je do drugiego konstruktora.

Drugi Konstruktor ma jeden parametr: parametr IMovieRepository. Ten konstruktor po prostu przypisuje wartość parametru do pola poziomu klasy o nazwie repozytorium \_.

Klasa MoviesController wykorzystuje Wzorzec projektowy oprogramowania nazywany wzorcem iniekcji zależności. W szczególności używa elementu wywoływanego jako iniekcja zależności konstruktora. Aby dowiedzieć się więcej na temat tego wzorca, przeczytaj następujący artykuł Fowlera:

[http://martinfowler.com/articles/injection.html](http://martinfowler.com/articles/injection.html)

Zwróć uwagę, że cały kod w klasie MoviesController (z wyjątkiem pierwszego konstruktora) współdziała z interfejsem IMovieRepository zamiast rzeczywistej klasy MovieRepository. Kod współdziała z interfejsem abstrakcyjnym, a nie z konkretną implementacją interfejsu.

Jeśli chcesz zmodyfikować technologię dostępu do danych używaną przez aplikację, możesz po prostu zaimplementować interfejs IMovieRepository z klasą, która korzysta z alternatywnej technologii dostępu do bazy danych. Na przykład można utworzyć klasę EntityFrameworkMovieRepository lub klasę SubSonicMovieRepository. Ponieważ klasa kontrolera jest zaprogramowana względem interfejsu, można przekazać nową implementację IMovieRepository do klasy Controller, a Klasa nadal będzie działała.

Ponadto, jeśli chcesz przetestować klasę MoviesController, możesz przekazać niesfałszowaną klasę repozytorium filmów do MoviesController. Można zaimplementować klasę IMovieRepository z klasą, która nie uzyskuje dostępu do bazy danych, ale zawiera wszystkie wymagane metody interfejsu IMovieRepository. Dzięki temu można jednostkowo testować klasę MoviesController bez faktycznego uzyskiwania dostępu do rzeczywistej bazy danych.

## <a name="summary"></a>Podsumowanie

Celem tego samouczka było zademonstrowanie sposobu tworzenia klas modelu MVC przez skorzystanie z zalet LINQ to SQL firmy Microsoft. Zbadamy dwie strategie wyświetlania danych bazy danych w aplikacji ASP.NET MVC. Najpierw tworzymy klasy LINQ to SQL i używały klas bezpośrednio w ramach akcji kontrolera. Używanie LINQ to SQL klas w ramach kontrolera umożliwia szybkie i łatwe wyświetlanie danych bazy danych w aplikacji MVC.

Następnie zbadamy nieco trudniejsze, ale jeszcze więcej składa do wyświetlania danych bazy danych. Wykorzystano wzorzec repozytorium i umieszczono całą logikę dostępu do bazy danych w osobnej klasie repozytorium. Nasz kontroler zapisał cały kod w porównaniu do interfejsu zamiast konkretnej klasy. Zaletą wzorca repozytorium jest umożliwienie nam zmiany technologii dostępu do bazy danych w przyszłości i umożliwia nam przetestowanie klas kontrolera.

> [!div class="step-by-step"]
> [Poprzednie](creating-model-classes-with-the-entity-framework-vb.md)
> [dalej](displaying-a-table-of-database-data-vb.md)

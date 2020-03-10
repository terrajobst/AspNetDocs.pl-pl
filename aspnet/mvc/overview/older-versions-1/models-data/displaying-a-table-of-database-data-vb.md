---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
title: Wyświetlanie tabeli danych bazy danych (VB) | Microsoft Docs
author: microsoft
description: W tym samouczku przedstawiono dwie metody wyświetlania zestawu rekordów bazy danych. Pokazujemy dwie metody formatowania zestawu rekordów bazy danych w HTML...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: 5bb4587f-5bcd-44f5-b368-3c1709162b35
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
msc.type: authoredcontent
ms.openlocfilehash: f2e2489ac8455913f55c746dbe05b9fe8272285b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78543016"
---
# <a name="displaying-a-table-of-database-data-vb"></a>Wyświetlanie tabeli danych bazy danych (VB)

przez [firmę Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](https://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_VB.pdf)

> W tym samouczku przedstawiono dwie metody wyświetlania zestawu rekordów bazy danych. Pokazujemy dwie metody formatowania zestawu rekordów bazy danych w tabeli HTML. Po pierwsze pokazuje, jak można formatować rekordy bazy danych bezpośrednio w widoku. Następnie pokazuje, jak można wykorzystać części częściowe podczas formatowania rekordów bazy danych.

Celem tego samouczka jest wyjaśnienie, jak można wyświetlić tabelę HTML danych bazy danych w aplikacji ASP.NET MVC. Najpierw dowiesz się, jak używać narzędzi do tworzenia szkieletów zawartych w programie Visual Studio, aby generować widok, który automatycznie wyświetla zestaw rekordów. Następnie dowiesz się, jak używać częściowej jako szablonu podczas formatowania rekordów bazy danych.

## <a name="create-the-model-classes"></a>Tworzenie klas modelu

Będziemy wyświetlać zestaw rekordów z tabeli bazy danych filmów. Tabela bazy danych filmów zawiera następujące kolumny:

<a id="0.4_table01"></a>

| **Nazwa kolumny** | **Typ danych** | **Zezwalaj na wartości null** |
| --- | --- | --- |
| Identyfikator | int | Fałsz |
| Tytuł | Nvarchar(200) | Fałsz |
| Dyrektor ds. | NVarchar (50) | Fałsz |
| DateReleased | DateTime | Fałsz |

Aby przedstawić tabelę filmów w aplikacji ASP.NET MVC, musimy utworzyć klasę modelu. W tym samouczku użyjemy Entity Framework firmy Microsoft do tworzenia naszych klas modelu.

> [!NOTE] 
> 
> W tym samouczku użyjemy Entity Framework firmy Microsoft. Jednak ważne jest, aby zrozumieć, że można użyć różnych technologii do współdziałania z bazą danych z poziomu aplikacji ASP.NET MVC, w tym LINQ to SQL, NHibernate lub ADO.NET.

Wykonaj następujące kroki, aby uruchomić Kreatora Entity Data Model:

1. Kliknij prawym przyciskiem myszy folder modele w oknie Eksplorator rozwiązań i wybierz opcję menu **Dodaj, nowy element**.
2. Wybierz kategorię **dane** i wybierz szablon **Entity Data Model ADO.NET** .
3. Nadaj modelowi danych nazwę *MoviesDBModel. edmx* , a następnie kliknij przycisk **Dodaj** .

Po kliknięciu przycisku Dodaj zostanie wyświetlony Kreator Entity Data Model (patrz rysunek 1). Wykonaj następujące kroki, aby zakończyć działanie kreatora:

1. W kroku **Wybierz zawartość modelu** wybierz opcję **Generuj z bazy danych** .
2. W kroku **Wybierz połączenie danych** Użyj połączenia danych *MoviesDB. mdf* oraz nazwy *MoviesDBEntities* dla ustawień połączenia. Kliknij przycisk **Dalej**.
3. W kroku **Wybierz obiekty bazy danych** rozwiń węzeł tabele, a następnie wybierz tabelę filmy. Wprowadź *modele* przestrzeni nazw i kliknij przycisk **Zakończ** .

[![tworzenia klas LINQ to SQL](displaying-a-table-of-database-data-vb/_static/image1.jpg)](displaying-a-table-of-database-data-vb/_static/image1.png)

**Ilustracja 01**. tworzenie klas LINQ to SQL ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-table-of-database-data-vb/_static/image2.png))

Po ukończeniu pracy Kreatora Entity Data Model zostanie otwarty projektant Entity Data Model. Projektant powinien wyświetlić jednostkę filmów (patrz rysunek 2).

[![projektanta Entity Data Model](displaying-a-table-of-database-data-vb/_static/image2.jpg)](displaying-a-table-of-database-data-vb/_static/image3.png)

**Ilustracja 02**: Projektant Entity Data Model ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-a-table-of-database-data-vb/_static/image4.png))

Przed kontynuowaniem musimy wprowadzić jedną zmianę. Kreator danych jednostki generuje klasy modelu o nazwie *filmy* , które reprezentują tabelę bazy danych filmów. Ponieważ będziemy używać klasy filmów do reprezentowania określonego filmu, musimy zmodyfikować nazwę klasy jako *Movie* zamiast *filmów* (pojedynczo, a nie plural).

Kliknij dwukrotnie nazwę klasy na powierzchni projektanta i Zmień nazwę klasy z filmów na film. Po wprowadzeniu tej zmiany kliknij przycisk **Zapisz** (ikona dyskietki) w celu wygenerowania klasy filmu.

## <a name="create-the-movies-controller"></a>Tworzenie kontrolera filmów

Teraz, gdy mamy możliwość reprezentowania rekordów bazy danych, możemy utworzyć kontroler, który zwraca kolekcję filmów. W oknie Eksplorator rozwiązań programu Visual Studio kliknij prawym przyciskiem myszy folder controllers (kontrolery) i wybierz opcję menu **Dodaj, kontroler** (zobacz rysunek 3).

[![menu Dodaj kontroler](displaying-a-table-of-database-data-vb/_static/image3.jpg)](displaying-a-table-of-database-data-vb/_static/image5.png)

**Ilustracja 03**: menu Dodaj kontroler ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-a-table-of-database-data-vb/_static/image6.png))

Gdy pojawi się okno dialogowe **Dodaj kontroler** , wprowadź nazwę kontrolera MovieController (zobacz rysunek 4). Kliknij przycisk **Dodaj** , aby dodać nowy kontroler.

[![oknie dialogowym Dodawanie kontrolera](displaying-a-table-of-database-data-vb/_static/image4.jpg)](displaying-a-table-of-database-data-vb/_static/image7.png)

**Ilustracja 04**: okno dialogowe Dodawanie kontrolera ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-a-table-of-database-data-vb/_static/image8.png))

Musimy zmodyfikować akcję index () uwidocznioną przez kontroler filmu, aby zwracała zestaw rekordów bazy danych. Zmodyfikuj kontroler tak, aby wyglądał jak kontroler na liście 1.

**Lista 1 – Controllers\MovieController.vb**

[!code-vb[Main](displaying-a-table-of-database-data-vb/samples/sample1.vb)]

Na liście 1 Klasa MoviesDBEntities jest używana do reprezentowania bazy danych MoviesDB. Jednostki wyrażenia *. MovieSet. ToList — ()* zwraca zestaw wszystkich filmów z tabeli bazy danych filmów.

## <a name="create-the-view"></a>Utwórz widok

Najprostszym sposobem wyświetlania zestawu rekordów bazy danych w tabeli HTML jest skorzystanie z szkieletu zapewnianego przez program Visual Studio.

Skompiluj aplikację, wybierając opcję menu **kompilacja, Kompiluj rozwiązanie**. Musisz skompilować aplikację przed otwarciem okna dialogowego **Dodaj widok** lub klasy danych nie będą wyświetlane w oknie dialogowym.

Kliknij prawym przyciskiem myszy akcję indeks () i wybierz opcję menu **Dodaj widok** (patrz rysunek 5).

[![Dodawanie widoku](displaying-a-table-of-database-data-vb/_static/image5.jpg)](displaying-a-table-of-database-data-vb/_static/image9.png)

**Ilustracja 05**. Dodawanie widoku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-table-of-database-data-vb/_static/image10.png))

W oknie dialogowym **Dodawanie widoku** zaznacz pole wyboru z etykietą **Utwórz widok o jednoznacznie określonym typie**. Wybierz klasę filmu jako **klasę danych widoku**. Wybierz pozycję *Lista* jako **zawartość widoku** (zobacz rysunek 6). Wybranie tych opcji spowoduje wygenerowanie widoku o jednoznacznie określonym typie, który wyświetla listę filmów.

[![oknie dialogowym Dodawanie widoku](displaying-a-table-of-database-data-vb/_static/image6.jpg)](displaying-a-table-of-database-data-vb/_static/image11.png)

**Ilustracja 06**. okno dialogowe Dodawanie widoku ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-a-table-of-database-data-vb/_static/image12.png))

Po kliknięciu przycisku **Dodaj** widok w liście 2 zostanie wygenerowany automatycznie. Ten widok zawiera kod wymagany do iteracji w kolekcji filmów i wyświetlania każdej z właściwości filmu.

**Lista 2 — Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample2.aspx)]

Możesz uruchomić aplikację, wybierając opcję menu **Debuguj, Rozpocznij debugowanie** (lub naciskając klawisz F5). Uruchomienie aplikacji powoduje uruchomienie programu Internet Explorer. Jeśli przejdziesz do adresu URL/Movie, zobaczysz stronę na rysunku 7.

[![tabeli filmów](displaying-a-table-of-database-data-vb/_static/image7.jpg)](displaying-a-table-of-database-data-vb/_static/image13.png)

**Ilustracja 07**: tabela filmów ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-a-table-of-database-data-vb/_static/image14.png))

Jeśli nie chcesz niczego o wyglądzie siatki rekordów bazy danych na rysunku 7, możesz po prostu zmodyfikować widok indeksu. Na przykład można zmienić nagłówek *DateReleased* na *Data zwolnienia* , modyfikując widok indeksu.

## <a name="create-a-template-with-a-partial"></a>Tworzenie szablonu z częściową

Gdy widok jest zbyt skomplikowany, dobrym pomysłem jest rozpoczęcie dzielenia widoku na częściowe. Korzystanie ze stron częściowych sprawia, że Twoje widoki są łatwiejsze do zrozumienia i utrzymania. Utworzymy częściową, której będzie można użyć jako szablonu do formatowania każdego rekordu bazy danych filmów.

Wykonaj następujące kroki, aby utworzyć częściowy:

1. Kliknij prawym przyciskiem myszy folder Views\Movie i wybierz opcję menu **Dodaj widok**.
2. Zaznacz pole wyboru z etykietą *Utwórz widok częściowy (. ascx)* .
3. Nazwij część *MovieTemplate*.
4. Zaznacz pole wyboru z etykietą **Utwórz widok o jednoznacznie określonym typie**.
5. Wybierz pozycję film jako *klasę danych widoku*.
6. Wybierz opcję pusty jako *zawartość widoku*.
7. Kliknij przycisk **Dodaj** , aby dodać część częściową do projektu.

Po wykonaniu tych kroków zmodyfikuj MovieTemplate częściowy, aby wyglądać jak lista 3.

**Lista 3 — Views\Movie\MovieTemplate.ascx**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample3.aspx)]

Częściowa lista 3 zawiera szablon dla pojedynczego wiersza rekordów.

Widok zmodyfikowany indeks w liście 4 używa częściowej MovieTemplate.

**Lista 4 – Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample4.aspx)]

Widok w liście 4 zawiera pętlę dla każdej pętli, która iteruje przez wszystkie filmy. Dla każdego filmu MovieTemplate częściowa służy do formatowania filmu. MovieTemplate jest renderowany przez wywołanie metody pomocnika RenderPartial ().

Zmodyfikowany widok indeksu służy do renderowania tej samej tabeli HTML rekordów bazy danych. Jednak widok został znacznie uproszczony.

Metoda RenderPartial () różni się od większości innych metod pomocnika, ponieważ nie zwraca ciągu. W związku z tym należy wywołać metodę RenderPartial () przy użyciu &lt;% html. RenderPartial ()%&gt; zamiast &lt;% = html. RenderPartial ()%&gt;.

## <a name="summary"></a>Podsumowanie

Celem tego samouczka jest wyjaśnienie, jak można wyświetlić zestaw rekordów bazy danych w tabeli HTML. Najpierw wiesz już, jak zwrócić zestaw rekordów bazy danych z akcji kontrolera, wykorzystując Entity Framework firmy Microsoft. Następnie pokazano, jak używać szkieletu programu Visual Studio do generowania widoku, który wyświetla kolekcję elementów automatycznie. Wreszcie wiesz już, jak uprościć widok, korzystając ze częściowej. Wiesz już, jak używać częściowej jako szablonu, aby można było sformatować każdy rekord bazy danych.

> [!div class="step-by-step"]
> [Poprzednie](creating-model-classes-with-linq-to-sql-vb.md)
> [dalej](performing-simple-validation-vb.md)

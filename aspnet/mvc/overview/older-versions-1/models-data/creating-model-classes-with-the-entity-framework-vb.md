---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-vb
title: Tworzenie klas modelu za pomocą Entity Framework (VB) | Microsoft Docs
author: microsoft
description: W ramach tego samouczka nauczysz się używać ASP.NET MVC z Entity Framework Microsoft. Dowiesz się, jak za pomocą Kreatora jednostki utworzyć jednostkę ADO.NET...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: ff8322c9-12f3-4e24-aba6-a38046b9bb0d
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-vb
msc.type: authoredcontent
ms.openlocfilehash: f6c896c6f5f6d898ac6f99d5998fb29cb73bcb10
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78543331"
---
# <a name="creating-model-classes-with-the-entity-framework-vb"></a>Tworzenie klas modeli za pomocą programu Entity Framework (VB)

przez [firmę Microsoft](https://github.com/microsoft)

> W ramach tego samouczka nauczysz się używać ASP.NET MVC z Entity Framework Microsoft. Dowiesz się, jak za pomocą Kreatora jednostki utworzyć Entity Data Model ADO.NET. W ramach tego samouczka utworzymy aplikację sieci Web, która ilustruje sposób wybierania, wstawiania, aktualizowania i usuwania danych bazy danych przy użyciu Entity Framework.

Celem tego samouczka jest wyjaśnienie, jak można tworzyć klasy dostępu do danych za pomocą Entity Framework firmy Microsoft podczas kompilowania aplikacji ASP.NET MVC. W tym samouczku nie założono żadnej wcześniejszej znajomości Entity Framework firmy Microsoft. Na końcu tego samouczka dowiesz się, jak używać Entity Framework do wybierania, wstawiania, aktualizowania i usuwania rekordów bazy danych.

Microsoft Entity Framework to narzędzie do mapowania relacyjnego obiektów (O/RM), które umożliwia automatyczne generowanie warstwy dostępu do danych z bazy danych. Entity Framework pozwala uniknąć żmudnym pracy nad tworzeniem klas dostępu do danych.

> [!NOTE] 
> 
> Nie istnieje podstawowe połączenie między ASP.NET MVC a Entity Framework Microsoft. Istnieje kilka alternatyw dla Entity Framework, których można użyć z ASP.NET MVC. Na przykład można skompilować klasy modelu MVC przy użyciu innych narzędzi O/RM, takich jak Microsoft LINQ to SQL, NHibernate lub Subsonic.

Aby zilustrować, jak można użyć Entity Framework firmy Microsoft z ASP.NET MVC, utworzymy prostą przykładową aplikację. Utworzymy aplikację bazy danych filmów, która umożliwia wyświetlanie i edytowanie rekordów bazy danych filmów.

W tym samouczku założono, że masz program Visual Studio 2008 lub Visual Web Developer 2008 z dodatkiem Service Pack 1. Aby można było użyć Entity Framework, wymagany jest dodatek Service Pack 1. Program Visual Studio 2008 Service Pack 1 lub Visual Web Developer z dodatkiem Service Pack 1 można pobrać z następującego adresu:

> [https://www.asp.net/downloads/](https://www.asp.net/downloads)

## <a name="creating-the-movie-sample-database"></a>Tworzenie przykładowej bazy danych filmu

Aplikacja do odtwarzania filmów korzysta z tabeli bazy danych o nazwie filmy, która zawiera następujące kolumny:

| Nazwa kolumny | Typ danych | Zezwolić na wartości null? | Czy jest kluczem podstawowym? |
| --- | --- | --- | --- |
| Identyfikator | int | Fałsz | True |
| Tytuł | nvarchar (100) | Fałsz | Fałsz |
| Dyrektor ds. | nvarchar (100) | Fałsz | Fałsz |

Tabelę można dodać do projektu ASP.NET MVC, wykonując następujące czynności:

1. Kliknij prawym przyciskiem myszy folder\_danych aplikacji w oknie Eksplorator rozwiązań i wybierz opcję menu **Dodaj, nowy element.**
2. W oknie dialogowym **Dodaj nowy element** wybierz pozycję **baza danych SQL Server**, nadaj bazie danych nazwę MoviesDB. mdf, a następnie kliknij przycisk **Dodaj** .
3. Kliknij dwukrotnie plik MoviesDB. mdf, aby otworzyć okno Eksplorator serwera/Eksplorator bazy danych.
4. Rozwiń połączenie z bazą danych MoviesDB. mdf, kliknij prawym przyciskiem myszy folder Tables i wybierz opcję menu **Dodaj nową tabelę**.
5. W Projektancie tabel Dodaj identyfikatory, tytuły i kolumny dyrektora.
6. Kliknij przycisk **Zapisz** (ma ikonę dyskietki), aby zapisać nową tabelę z nazwami filmów.

Po utworzeniu tabeli bazy danych filmów należy dodać do niej przykładowe dane. Kliknij prawym przyciskiem myszy tabelę filmy i wybierz opcję menu **Pokaż dane tabeli**. Możesz wprowadzić fałszywe dane filmu do wyświetlonej siatki.

## <a name="creating-the-adonet-entity-data-model"></a>Tworzenie Entity Data Model ADO.NET

Aby można było użyć Entity Framework, należy utworzyć Entity Data Model. Aby automatycznie wygenerować Entity Data Model z bazy danych, można skorzystać z *kreatora Entity Data Model* programu Visual Studio.

Wykonaj następujące kroki:

1. Kliknij prawym przyciskiem myszy folder modele w oknie Eksplorator rozwiązań i wybierz opcję menu **Dodaj, nowy element**.
2. W oknie dialogowym **Dodaj nowy element** wybierz kategorię dane (patrz rysunek 1).
3. Wybierz szablon **Entity Data Model ADO.NET** , nadaj Entity Data Model nazwę MoviesDBModel. edmx, a następnie kliknij przycisk **Dodaj** . Kliknięcie przycisku **Dodaj** powoduje uruchomienie Kreatora modelu danych.
4. W kroku **Wybierz zawartość modelu** wybierz opcję **Generuj z bazy danych** , a następnie kliknij przycisk **dalej** (patrz rysunek 2).
5. W kroku **Wybierz połączenie danych** wybierz połączenie z bazą danych MoviesDB. mdf, wprowadź nazwę ustawienia połączenia jednostki MoviesDBEntities, a następnie kliknij przycisk **dalej** (zobacz rysunek 3).
6. W kroku **Wybierz obiekty bazy danych** wybierz tabelę programu Movie Database i kliknij przycisk **Zakończ** (zobacz rysunek 4).

Po wykonaniu tych kroków zostanie otwarty program ADO.NET Entity Data Model Designer (Entity Designer).

**Rysunek 1 — Tworzenie nowego Entity Data Model**

![clip_image002](creating-model-classes-with-the-entity-framework-vb/_static/image1.jpg)

**Rysunek 2 — Wybieranie etapu zawartości modelu**

![clip_image004](creating-model-classes-with-the-entity-framework-vb/_static/image2.jpg)

**Rysunek 3 — Wybieranie połączenia danych**

![clip_image006](creating-model-classes-with-the-entity-framework-vb/_static/image3.jpg)

**Rysunek 4 — Wybieranie obiektów bazy danych**

![clip_image008](creating-model-classes-with-the-entity-framework-vb/_static/image4.jpg)

## <a name="modifying-the-adonet-entity-data-model"></a>Modyfikowanie Entity Data Model ADO.NET

Po utworzeniu Entity Data Model można zmodyfikować model, korzystając z Entity Designer (patrz rysunek 5). Entity Designer można otworzyć w dowolnym momencie, klikając dwukrotnie plik MoviesDBModel. edmx znajdujący się w folderze models w oknie Eksplorator rozwiązań.

**Rysunek 5 — Projektant Entity Data Model ADO.NET**

![clip_image010](creating-model-classes-with-the-entity-framework-vb/_static/image5.jpg)

Na przykład można użyć Entity Designer, aby zmienić nazwy klas generowanych przez kreatora danych modelu jednostki. Kreator utworzył nową klasę dostępu do danych o nazwie filmy. Innymi słowy, Kreator wydał klasę o takiej samej nazwie jak tabela bazy danych. Ponieważ ta klasa zostanie użyta do reprezentowania określonego wystąpienia filmu, należy zmienić nazwę klasy z filmów na film.

Jeśli chcesz zmienić nazwę klasy jednostki, możesz kliknąć dwukrotnie nazwę klasy w Entity Designer i wprowadzić nową nazwę (patrz rysunek 6). Alternatywnie można zmienić nazwę jednostki w okno Właściwości po wybraniu jednostki w Entity Designer.

**Rysunek 6 — Zmiana nazwy jednostki**

![clip_image012](creating-model-classes-with-the-entity-framework-vb/_static/image6.jpg)

Pamiętaj, aby zapisać Entity Data Model po wprowadzeniu modyfikacji, klikając przycisk Zapisz (ikonę dyskietki). W tle Entity Designer generuje zestaw Visual Basic klas platformy .NET. Można wyświetlić te klasy, otwierając plik MoviesDBModel. Designer. vb z okna Eksplorator rozwiązań.

Nie należy modyfikować kodu w pliku Designer. vb, ponieważ zmiany zostaną nadpisywane przy następnym użyciu Entity Designer. Jeśli chcesz zwiększyć funkcjonalność klas jednostek zdefiniowanych w pliku Designer. vb, możesz utworzyć *klasy częściowe* w oddzielnych plikach.

#### <a name="selecting-database-records-with-the-entity-framework"></a>Wybieranie rekordów bazy danych z Entity Framework

Rozpocznijmy Tworzenie aplikacji bazy danych filmów, tworząc stronę, która wyświetla listę rekordów filmów. Kontroler Home na liście 1 uwidacznia akcję o nazwie index (). Akcja index () zwraca wszystkie rekordy filmu z tabeli bazy danych filmów, wykorzystując Entity Framework.

**Lista 1 – Controllers\HomeController.vb**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample1.vb)]

Należy zauważyć, że kontroler z listą 1 zawiera konstruktora. Konstruktor inicjuje pole na poziomie klasy o nazwie \_DB. Pole \_DB reprezentuje jednostki bazy danych wygenerowane przez Entity Framework Microsoft. Pole \_DB jest wystąpieniem klasy MoviesDBEntities, która została wygenerowana przez Entity Designer.

Pole \_DB jest używane w ramach akcji index () do pobierania rekordów z tabeli bazy danych filmów. Wyrażenie \_DB. MovieSet reprezentuje wszystkie rekordy z tabeli bazy danych filmów. Metoda ToList — () służy do konwertowania zestawu filmów na ogólną kolekcję obiektów filmów: list (film).

Rekordy filmów są pobierane z pomocą LINQ to Entities. W akcji index () na liście 1 użyto *składni metody* LINQ do pobrania zestawu rekordów bazy danych. Jeśli wolisz, możesz zamiast tego użyć *składni zapytania* LINQ. Poniższe dwie instrukcje wykonują takie same czynności:

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample2.vb)]

Używaj niezależnej składni LINQ — składni metody lub składni zapytania — jest to najbardziej intuicyjne. Nie ma różnicy w wydajności między dwoma podejściami — jedyną różnicą jest styl.

Widok w liście 2 służy do wyświetlania rekordów filmów.

**Lista 2 — Views\Home\Index.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample3.aspx)]

Widok na liście 2 zawiera pętlę **dla każdej** pętli, która iteruje przez każdy rekord filmu i wyświetla wartości tytułu i właściwości tego rekordu filmu. Zwróć uwagę, że obok każdego rekordu jest wyświetlany link Edytuj i Usuń. Ponadto w dolnej części widoku pojawi się link Dodaj film (patrz rysunek 7).

**Rysunek 7 — Widok indeksu**

![clip_image014](creating-model-classes-with-the-entity-framework-vb/_static/image7.jpg)

Widok indeksu jest widokiem o *określonym typie*. Widok indeksu ma &lt;% @ Page%&gt; dyrektywy, która zawiera atrybut Inherits. Atrybut Inherits rzutuje Właściwość ViewData. model na kolekcję listy ogólnej o jednoznacznie określonym typie obiektów filmów — listę (film).

## <a name="inserting-database-records-with-the-entity-framework"></a>Wstawianie rekordów bazy danych z Entity Framework

Za pomocą Entity Framework można ułatwić wstawianie nowych rekordów do tabeli bazy danych. Lista 3 zawiera dwie nowe akcje dodane do klasy kontrolera macierzystego, za pomocą których można wstawiać nowe rekordy do tabeli bazy danych filmów.

**Lista 3 – Controllers\HomeController.vb (dodawanie metod)**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample4.vb)]

Pierwsza akcja Dodaj () po prostu zwraca widok. Widok zawiera formularz służący do dodawania nowego rekordu bazy danych filmów (patrz rysunek 8). Po przesłaniu formularza zostaje wywołana druga akcja Dodaj ().

Zwróć uwagę, że druga akcja Dodaj () ma atrybut AcceptVerbs. Tę akcję można wywołać tylko w przypadku wykonywania operacji POST protokołu HTTP. Innymi słowy, ta akcja może być wywoływana tylko w przypadku ogłaszania formularza HTML.

Druga akcja Add () tworzy nowe wystąpienie klasy filmu Entity Framework z pomocą metody ASP.NET MVC TryUpdateModel (). Metoda TryUpdateModel () przyjmuje pola w Postacicollection przekazaną do metody Add () i przypisuje wartości tych pól formularza HTML do klasy Movie.

W przypadku korzystania z Entity Framework należy podać "białą listę" właściwości przy użyciu metod TryUpdateModel lub UpdateModel do aktualizowania właściwości klasy jednostki.

Następnie akcja Dodaj () wykonuje kilka prostych walidacji formularza. Akcja weryfikuje, czy zarówno tytuł, jak i właściwości dyrektora mają wartości. Jeśli wystąpi błąd walidacji, zostanie dodany komunikat o błędzie weryfikacji do ModelState.

Jeśli nie występują błędy walidacji, do tabeli bazy danych filmów zostanie dodany nowy rekord filmu z pomocą Entity Framework. Nowy rekord zostanie dodany do bazy danych z następującymi dwoma wierszami kodu:

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample5.vb)]

Pierwszy wiersz kodu dodaje nową jednostkę filmu do zestawu filmów śledzonych przez Entity Framework. Drugi wiersz kodu zapisuje wszelkie zmiany w filmach śledzonych z powrotem do podstawowej bazy danych.

**Rysunek 8 — widok dodawania**

![clip_image016](creating-model-classes-with-the-entity-framework-vb/_static/image8.jpg)

## <a name="updating-database-records-with-the-entity-framework"></a>Aktualizowanie rekordów bazy danych za pomocą Entity Framework

Można przestrzegać niemal tego samego podejścia do edytowania rekordu bazy danych przy użyciu Entity Framework jako podejścia, które właśnie zastosowano do wstawienia nowego rekordu bazy danych. Lista 4 zawiera dwie nowe akcje kontrolera o nazwie Edit (). Pierwsza akcja Edytuj () zwraca formularz HTML służący do edytowania rekordu filmu. Druga akcja Edytuj () podejmuje próbę zaktualizowania bazy danych.

**Lista 4 – Controllers\HomeController.vb (edycja metod)**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample6.vb)]

Druga akcja Edit () zaczyna się od pobrania rekordu filmu z bazy danych, która pasuje do identyfikatora edytowanego filmu. Poniższa instrukcja LINQ to Entities umożliwia pobranie pierwszego rekordu bazy danych zgodnego z określonym identyfikatorem:

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample7.vb)]

Następnie Metoda TryUpdateModel () służy do przypisywania wartości pól formularza HTML do właściwości jednostki filmu. Zwróć uwagę, że podano białą listę, aby określić dokładne właściwości do zaktualizowania.

Kolejne proste sprawdzanie poprawności jest wykonywane w celu sprawdzenia, czy zarówno tytuł filmu, jak i właściwości dyrektora mają wartości. Jeśli żadna właściwość nie zawiera wartości, do ModelState i ModelState. IsValid zwraca wartość false.

Na koniec, jeśli nie występują błędy walidacji, źródłowa tabela bazy danych filmów jest aktualizowana o wszelkie zmiany przez wywołanie metody metody SaveChanges ().

Podczas edytowania rekordów bazy danych należy przekazać identyfikator edytowanego rekordu do akcji kontrolera, która wykonuje aktualizację bazy danych. W przeciwnym razie akcja kontrolera nie będzie wiedzieć, który rekord należy zaktualizować w źródłowej bazie danych. Widok edycji zawarty w liście 5 zawiera ukryte pole formularza, które reprezentuje identyfikator edytowanego rekordu bazy danych.

**Lista 5 – Views\Home\Edit.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample8.aspx)]

## <a name="deleting-database-records-with-the-entity-framework"></a>Usuwanie rekordów bazy danych za pomocą Entity Framework

Końcową operacją bazy danych, którą musimy wykonać w tym samouczku, jest usunięcie rekordów bazy danych. W celu usunięcia określonego rekordu bazy danych można użyć akcji kontrolera w temacie 6.

**Lista 6--\Controllers\HomeController.vb (Akcja usuwania)**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample9.vb)]

Akcja Usuń () najpierw pobiera jednostkę filmu, która pasuje do identyfikatora przesłanego do akcji. Następnie film zostanie usunięty z bazy danych przez wywołanie metody DeleteObject (), po której następuje Metoda metody SaveChanges (). Na koniec użytkownik zostanie przekierowany z powrotem do widoku indeksu.

## <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono sposób tworzenia aplikacji sieci Web opartych na bazie danych dzięki wykorzystaniu ASP.NET MVC i Microsoft Entity Framework. Wiesz już, jak utworzyć aplikację, która umożliwia wybieranie, wstawianie, aktualizowanie i usuwanie rekordów bazy danych.

Najpierw omówiono, jak za pomocą Kreatora Entity Data Model wygenerować Entity Data Model z poziomu programu Visual Studio. Następnie dowiesz się, jak za pomocą LINQ to Entities pobrać zestaw rekordów bazy danych z tabeli bazy danych. Na koniec użyto Entity Framework do wstawiania, aktualizowania i usuwania rekordów bazy danych.

> [!div class="step-by-step"]
> [Poprzednie](validation-with-the-data-annotation-validators-cs.md)
> [dalej](creating-model-classes-with-linq-to-sql-vb.md)

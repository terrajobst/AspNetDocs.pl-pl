---
uid: mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb
title: Tworzenie aplikacji bazy danych filmów w ciągu 15 minut za pomocą ASP.NET MVC (VB) | Microsoft Docs
author: StephenWalther
description: Stephen Walther kompiluje całą aplikację MVC opartą na bazie danych ASP.NET od początku do końca. Ten samouczek jest doskonałym wprowadzeniem dla osób, które są nowym t...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: e4ba9786-734c-4eb3-91bb-089793325d0d
msc.legacyurl: /mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb
msc.type: authoredcontent
ms.openlocfilehash: 0ce8161d29a8ab4005e2b20462b08c9e10ee815a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78541889"
---
# <a name="create-a-movie-database-application-in-15-minutes-with-aspnet-mvc-vb"></a>Tworzenie aplikacji bazy danych filmów w ciągu 15 minut za pomocą wzorca ASP.NET MVC (VB)

Autor [Stephen Walther](https://github.com/StephenWalther)

[Pobierz kod](https://download.microsoft.com/download/7/2/8/728F8794-E59A-4D18-9A56-7AD2DB05BD9D/MovieApp_VB.zip)

> Stephen Walther kompiluje całą aplikację MVC opartą na bazie danych ASP.NET od początku do końca. Ten samouczek to doskonałe wprowadzenie do osób, które są nowe dla struktury ASP.NET MVC i które chcą uzyskać zrozumienie procesu tworzenia aplikacji ASP.NET MVC.

Celem tego samouczka jest poznanie "czego lubisz" w celu utworzenia aplikacji ASP.NET MVC. W tym samouczku tworzymy cały proces tworzenia całej aplikacji ASP.NET MVC od początku do końca. Pokazuje, jak utworzyć prostą aplikację opartą na bazie danych, która ilustruje sposób wyświetlania, tworzenia i edytowania rekordów bazy danych.

Aby uprościć proces kompilowania aplikacji, będziemy korzystać z funkcji tworzenia szkieletów w programie Visual Studio 2008. Zezwolimy programowi Visual Studio na generowanie początkowego kodu i zawartości dla naszych kontrolerów, modeli i widoków.

Jeśli pracujesz z witrynami Active Server lub ASP.NET, należy dobrze znaleźć ASP.NET MVC. Widoki ASP.NET MVC są bardzo podobne do stron w aplikacji Active Server Pages. Podobnie jak w przypadku tradycyjnych aplikacji ASP.NET Web Forms, ASP.NET MVC zapewnia pełny dostęp do bogatego zestawu języków i klas dostarczanych przez program .NET Framework.

Mamy nadzieję, że ten samouczek zapewni Ci zrozumienie, jak środowisko tworzenia aplikacji ASP.NET MVC jest podobne i różne niż środowisko tworzenia stron Active Server lub aplikacji ASP.NET Web Forms.

## <a name="overview-of-the-movie-database-application"></a>Przegląd aplikacji bazy danych filmów

Ponieważ naszym celem jest zachowanie prostoty, utworzymy bardzo prostą aplikację bazy danych filmów. Nasza prosta aplikacja do baz danych filmów umożliwi nam wykonywanie trzech czynności:

1. Wyświetlanie zestawu rekordów bazy danych filmów
2. Utwórz nowy rekord bazy danych filmów
3. Edytowanie istniejącego rekordu bazy danych filmów

Ponownie, ponieważ chcemy zachować prostotę, będziemy korzystać z minimalnej liczby funkcji platformy ASP.NET MVC potrzebnych do kompilowania aplikacji. Na przykład nie będziemy używać programowania opartego na testach.

Aby można było utworzyć aplikację, należy wykonać następujące czynności:

1. Tworzenie projektu aplikacji sieci Web ASP.NET MVC
2. Tworzenie bazy danych
3. Tworzenie modelu bazy danych
4. Tworzenie kontrolera ASP.NET MVC
5. Tworzenie widoków ASP.NET MVC

## <a name="preliminaries"></a>Akcje wstępne

Do skompilowania aplikacji ASP.NET MVC wymagany jest program Visual Studio 2008 lub Visual Web Developer 2008 Express. Należy również pobrać strukturę ASP.NET MVC.

Jeśli nie jesteś członkiem programu Visual Studio 2008, możesz pobrać 90-dniową wersję próbną programu Visual Studio 2008 z tej witryny sieci Web:

[https://msdn.microsoft.com/vs2008/products/cc268305.aspx](https://msdn.microsoft.com/vs2008/products/cc268305.aspx)

Alternatywnie można tworzyć aplikacje ASP.NET MVC przy użyciu programu Visual Web Developer Express 2008. Jeśli zdecydujesz się użyć programu Visual Web Developer Express, musisz mieć zainstalowany dodatek Service Pack 1. Program Visual Web Developer 2008 Express z dodatkiem Service Pack 1 można pobrać z tej witryny sieci Web:

[https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;d isplaylang = pl](https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang=en)

Po zainstalowaniu programu Visual Studio 2008 lub Visual Web Developer 2008 należy zainstalować platformę ASP.NET MVC. Strukturę ASP.NET MVC można pobrać z następującej witryny sieci Web:

[https://www.asp.net/mvc/](../../../index.md)

> [!NOTE] 
> 
> Zamiast pobierać ASP.NET Framework i ASP.NET MVC Framework osobno, można skorzystać z Instalatora platformy sieci Web. Instalator platformy sieci Web to aplikacja, która umożliwia łatwe zarządzanie zainstalowanymi aplikacjami:
> 
> [https://www.microsoft.com/web/gallery/Install.aspx](https://www.microsoft.com/web/gallery/Install.aspx)

## <a name="creating-an-aspnet-mvc-web-application-project"></a>Tworzenie projektu aplikacji sieci Web ASP.NET MVC

Zacznijmy od utworzenia nowego projektu aplikacji sieci Web ASP.NET MVC w programie Visual Studio 2008. Wybierz plik opcji menu **, nowy projekt** i zobaczysz okno dialogowe Nowy projekt na rysunku 1. Wybierz Visual Basic jako język programowania i wybierz szablon projektu aplikacji sieci Web ASP.NET MVC. Nadaj projektowi nazwę MovieApp i kliknij przycisk OK.

[![okno dialogowe Nowy projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image1.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image1.png)

**Ilustracja 01**. okno dialogowe Nowy projekt ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image2.png))

Upewnij się, że wybrano pozycję .NET Framework 3,5 z listy rozwijanej w górnej części okna dialogowego Nowy projekt lub szablon projektu aplikacji sieci Web ASP.NET MVC nie zostanie wyświetlony.

Za każdym razem, gdy tworzysz nowy projekt aplikacji sieci Web MVC, Visual Studio poprosi o utworzenie oddzielnego projektu testu jednostkowego. Zostanie wyświetlone okno dialogowe na rysunku 2. Ponieważ nie będziemy tworzyć testów w tym samouczku ze względu na ograniczenia czasowe (i, tak, należy zastanowić się na to trochę winno) wybierz opcję **nie** , a następnie kliknij przycisk **OK** .

> [!NOTE] 
> 
> Visual Web Developer nie obsługuje projektów testowych.

[![okno dialogowe Nowy projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image2.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image3.png)

**Ilustracja 02**. okno dialogowe Tworzenie projektu testu jednostkowego ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image4.png))

Aplikacja ASP.NET MVC ma standardowy zestaw folderów: model, widoki i folder controllers. Ten standardowy zestaw folderów można zobaczyć w oknie Eksplorator rozwiązań. Należy dodać pliki do każdego z folderów modele, widoki i kontrolery, aby skompilować aplikację bazy danych filmów.

Gdy tworzysz nową aplikację MVC przy użyciu programu Visual Studio, uzyskasz przykładową aplikację. Ponieważ chcemy zacząć od podstaw, musimy usunąć zawartość tej przykładowej aplikacji. Należy usunąć następujący plik i następujący folder:

- Controllers\HomeController.vb
- Views\Home

## <a name="creating-the-database"></a>Tworzenie bazy danych

Musimy utworzyć bazę danych do przechowywania rekordów bazy danych filmów. Na szczęście, program Visual Studio zawiera bezpłatną bazę danych o nazwie SQL Server Express. Wykonaj następujące kroki, aby utworzyć bazę danych:

1. Kliknij prawym przyciskiem myszy folder\_danych aplikacji w oknie Eksplorator rozwiązań i wybierz opcję menu **Dodaj, nowy element**.
2. Wybierz kategorię **dane** i wybierz szablon **bazy danych SQL Server** (zobacz rysunek 3).
3. Nazwij nową bazę danych *MoviesDB. mdf* i kliknij przycisk **Dodaj** .

Po utworzeniu bazy danych można nawiązać połączenie z bazą danych, klikając dwukrotnie plik MoviesDB. mdf znajdujący się w folderze danych\_aplikacji. Dwukrotne kliknięcie pliku MoviesDB. mdf spowoduje otwarcie okna Eksplorator serwera.

> [!NOTE] 
> 
> Okno Eksplorator serwera ma nazwę okna Eksplorator bazy danych w przypadku Visual Web Developer.

[![okno dialogowe Nowy projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image3.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image5.png)

**Ilustracja 03**: Tworzenie bazy danych Microsoft SQL Server ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image6.png))

Następnie musimy utworzyć nową tabelę bazy danych. W oknie Eksploratora serwera kliknij prawym przyciskiem myszy folder tabele i wybierz opcję menu **Dodaj nową tabelę**. Wybranie tej opcji menu spowoduje otwarcie projektanta tabel bazy danych. Utwórz następujące kolumny bazy danych:

<a id="0.2_table01"></a>

| **Nazwa kolumny** | **Typ danych** | **Zezwalaj na wartości null** |
| --- | --- | --- |
| Identyfikator | int | Fałsz |
| Tytuł | Nvarchar(100) | Fałsz |
| Dyrektor ds. | Nvarchar(100) | Fałsz |
| DateReleased | DateTime | Fałsz |

Pierwsza kolumna, Identyfikator kolumny, ma dwie specjalne właściwości. Najpierw należy oznaczyć kolumnę ID jako kolumnę klucza podstawowego. Po wybraniu kolumny Identyfikator kliknij przycisk **Ustaw klucz podstawowy** (jest to ikona, która wygląda jak klucz). Następnie należy oznaczyć kolumnę ID jako kolumnę tożsamości. W kolumnie okno Właściwości przewiń w dół do sekcji Specyfikacja tożsamości i rozwiń ją. Zmień właściwość **jest tożsamości** na wartość **tak**. Po zakończeniu tabela powinna wyglądać jak rysunek 4.

[![okno dialogowe Nowy projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image4.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image7.png)

**Ilustracja 04**: tabela bazy danych filmów ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image8.png))

Ostatnim krokiem jest zapisanie nowej tabeli. Kliknij przycisk Zapisz (ikona dyskietki) i nadaj nowej tabeli nazwę filmy.

Po zakończeniu tworzenia tabeli Dodaj do niej rekordy filmów. Kliknij prawym przyciskiem myszy tabelę filmy w oknie Eksplorator serwera i wybierz opcję menu **Pokaż dane tabeli**. Wprowadź listę ulubionych filmów (patrz rysunek 5).

[![okno dialogowe Nowy projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image5.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image9.png)

**Ilustracja 05**. wprowadzanie rekordów filmów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image10.png))

## <a name="creating-the-model"></a>Tworzenie modelu

Dalej musimy utworzyć zestaw klas, aby reprezentować naszą bazę danych. Musimy utworzyć model bazy danych. Będziemy korzystać z Entity Framework firmy Microsoft w celu automatycznego wygenerowania klas dla naszego modelu bazy danych.

> [!NOTE] 
> 
> Struktura ASP.NET MVC nie jest powiązana z Entity Frameworkem firmy Microsoft. Klasy modelu bazy danych można utworzyć, korzystając z różnych narzędzi mapowania relacyjnego obiektów (lub/M), w tym LINQ to SQL, poddźwięku i NHibernate.

Wykonaj następujące kroki, aby uruchomić Kreatora Entity Data Model:

1. Kliknij prawym przyciskiem myszy folder modele w oknie Eksplorator rozwiązań i wybierz opcję menu **Dodaj, nowy element**.
2. Wybierz kategorię **dane** i wybierz szablon **Entity Data Model ADO.NET** .
3. Nadaj modelowi danych nazwę *MoviesDBModel. edmx* , a następnie kliknij przycisk **Dodaj** .

Po kliknięciu przycisku Dodaj zostanie wyświetlony Kreator Entity Data Model (zobacz rysunek 6). Wykonaj następujące kroki, aby zakończyć działanie kreatora:

1. W kroku **Wybierz zawartość modelu** wybierz opcję **Generuj z bazy danych** .
2. W kroku **Wybierz połączenie danych** Użyj połączenia danych *MoviesDB. mdf* oraz nazwy *MoviesDBEntities* dla ustawień połączenia. Kliknij przycisk **Dalej**.
3. W kroku **Wybierz obiekty bazy danych** rozwiń węzeł tabele, a następnie wybierz tabelę filmy. Wprowadź przestrzeń nazw *MovieApp. models* i kliknij przycisk **Zakończ** .

[![okno dialogowe Nowy projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image6.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image11.png)

**Ilustracja 06**. Generowanie modelu bazy danych za pomocą Kreatora Entity Data Model ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image12.png))

Po ukończeniu pracy Kreatora Entity Data Model zostanie otwarty projektant Entity Data Model. Projektant powinien wyświetlić tabelę bazy danych filmów (patrz rysunek 7).

[![okno dialogowe Nowy projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image7.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image13.png)

**Ilustracja 07**: Projektant Entity Data Model ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image14.png))

Przed kontynuowaniem musimy wprowadzić jedną zmianę. Kreator danych jednostki generuje klasy modelu o nazwie filmy, które reprezentują tabelę bazy danych filmów. Ponieważ będziemy używać klasy filmów do reprezentowania określonego filmu, musimy zmodyfikować nazwę klasy jako *Movie* zamiast *filmów* (pojedynczo, a nie plural).

Kliknij dwukrotnie nazwę klasy na powierzchni projektanta i Zmień nazwę klasy z filmów na film. Po wprowadzeniu tej zmiany kliknij przycisk **Zapisz** (ikona dyskietki) w celu wygenerowania klasy filmu.

## <a name="creating-the-aspnet-mvc-controller"></a>Tworzenie kontrolera ASP.NET MVC

Następnym krokiem jest utworzenie kontrolera ASP.NET MVC. Kontroler jest odpowiedzialny za kontrolowanie sposobu, w jaki użytkownik współdziała z aplikacją ASP.NET MVC.

Wykonaj następujące kroki:

1. W oknie Eksplorator rozwiązań kliknij prawym przyciskiem myszy folder controllers, a następnie wybierz opcję menu **Dodaj, kontroler**.
2. W oknie dialogowym Dodawanie kontrolera wprowadź nazwę *HomeController* i zaznacz pole wyboru z etykietą **Dodaj metody akcji dla scenariuszy tworzenia, aktualizacji i szczegółów** (zobacz rysunek 8).
3. Kliknij przycisk **Dodaj** , aby dodać nowy kontroler do projektu.

Po wykonaniu tych kroków zostanie utworzony kontroler z listą 1. Należy zauważyć, że zawiera on metody o nazwie index, Details, Create i Edit. W poniższych sekcjach dodamy kod niezbędny do działania tych metod.

[![okno dialogowe Nowy projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image8.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image15.png)

**Ilustracja 08**: Dodawanie nowego kontrolera ASP.NET MVC ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image16.png))

**Lista 1 – Controllers\HomeController.vb**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample1.vb)]

## <a name="listing-database-records"></a>Wyświetlanie listy rekordów bazy danych

Metoda index () kontrolera macierzystego jest metodą domyślną dla aplikacji ASP.NET MVC. Po uruchomieniu aplikacji ASP.NET MVC, Metoda index () jest metodą pierwszego kontrolera, która jest wywoływana.

Użyjemy metody index (), aby wyświetlić listę rekordów z tabeli bazy danych filmów. Będziemy korzystać z klas modelu bazy danych utworzonych wcześniej w celu pobrania rekordów bazy danych filmów z użyciem metody index ().

Klasa HomeController została zmodyfikowana na liście 2, aby zawierała nowe pole prywatne o nazwie \_DB. Klasa MoviesDBEntities reprezentuje nasz model bazy danych i użyjemy tej klasy do komunikowania się z naszą bazą danych.

Zmodyfikowano również metodę index () na liście 2. Metoda index () używa klasy MoviesDBEntities w celu pobrania wszystkich rekordów filmów z tabeli bazy danych filmów. Wyrażenie *\_DB. MovieSet. ToList — ()* zwraca listę wszystkich rekordów filmów z tabeli bazy danych filmów.

Lista filmów jest przenoszona do widoku. Wszystkie elementy, które są przesyłane do metody View (), są przesyłane do widoku jako dane widoku.

**Lista 2 — controllers/HomeController. vb (Metoda zmodyfikowanego indeksu)**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample2.vb)]

Metoda index () zwraca widok o nazwie index. Musimy utworzyć ten widok, aby wyświetlić listę rekordów bazy danych filmów. Wykonaj następujące kroki:

Należy skompilować projekt (wybierz opcję menu **kompilacja, rozwiązanie kompilacji**) przed otwarciem okna dialogowego **Dodawanie widoku** lub żadne klasy nie pojawią się na liście rozwijanej **Klasa wyświetlania danych** .

1. Kliknij prawym przyciskiem myszy metodę index () w edytorze kodu i wybierz opcję menu **Dodaj widok** (patrz rysunek 9).
2. W oknie dialogowym Dodawanie widoku Sprawdź, czy pole wyboru z etykietą **Utwórz widok o jednoznacznie określonym typie** jest zaznaczone.
3. Z listy rozwijanej **Wyświetl zawartość** wybierz *listę*wartość.
4. Z listy rozwijanej **Wyświetl klasę danych** wybierz wartość *MovieApp. Movie*.
5. Kliknij przycisk Dodaj, aby utworzyć nowy widok (patrz rysunek 10).

Po wykonaniu tych kroków nowy widok o nazwie index. aspx zostanie dodany do folderu Views\Home. Zawartość widoku indeksu znajduje się na liście 3.

[![okno dialogowe Nowy projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image9.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image17.png)

**Ilustracja 09**. Dodawanie widoku z akcji kontrolera ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image18.png))

[![okno dialogowe Nowy projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image10.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image19.png)

**Ilustracja 10**. Tworzenie nowego widoku przy użyciu okna dialogowego Dodawanie widoku ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image20.png))

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample3.aspx)]

W widoku indeksu są wyświetlane wszystkie rekordy filmów z tabeli bazy danych filmów w tabeli HTML. Widok zawiera pętlę for each, która wykonuje iterację przez poszczególne filmy reprezentowane przez właściwość ViewData. model. Jeśli aplikacja zostanie uruchomiona przez naciśnięcie klawisza F5, zobaczysz stronę sieci Web na rysunku 11.

[![okno dialogowe Nowy projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image11.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image21.png)

**Ilustracja 11**. widok indeksu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image22.png))

## <a name="creating-new-database-records"></a>Tworzenie nowych rekordów bazy danych

Widok indeksu utworzony w poprzedniej sekcji zawiera link do tworzenia nowych rekordów bazy danych. Przejdźmy i zaimplementujmy logikę i utworzysz widok niezbędny do utworzenia nowych rekordów bazy danych filmów.

Kontroler Home zawiera dwie metody o nazwie Create (). Pierwsza metoda Create () nie ma parametrów. To Przeciążenie metody Create () służy do wyświetlania formularza HTML służącego do tworzenia nowego rekordu bazy danych filmu.

Druga metoda Create () ma parametr FormCollection. To Przeciążenie metody Create () jest wywoływana, gdy formularz HTML służący do tworzenia nowego filmu zostanie opublikowany na serwerze. Należy zauważyć, że ta druga metoda Create () ma atrybut AcceptVerbs, który uniemożliwia wywoływanie metody, chyba że zostanie wykonana operacja post protokołu HTTP.

Ta druga metoda Create () została zmodyfikowana w zaktualizowanej klasie HomeController na liście 4. Nowa wersja metody Create () akceptuje parametr filmu i zawiera logikę wstawiania nowego filmu do tabeli bazy danych filmów.

> [!NOTE] 
> 
> Zwróć uwagę na atrybut bind. Ponieważ nie chcemy zaktualizować właściwości identyfikatora filmu z formularza HTML, musimy jawnie wykluczyć tę właściwość.

**Lista 4 – Controllers\HomeController.vb (Zmodyfikowana metoda tworzenia)**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample4.vb)]

Program Visual Studio ułatwia tworzenie formularza do tworzenia nowego rekordu bazy danych filmów (patrz rysunek 12). Wykonaj następujące kroki:

1. Kliknij prawym przyciskiem myszy metodę Create () w edytorze kodu i wybierz opcję menu **Dodaj widok**.
2. Sprawdź, czy pole wyboru z etykietą **Utwórz widok o jednoznacznie określonym typie** jest zaznaczone.
3. Z listy rozwijanej **Wyświetl zawartość** wybierz pozycję *Utwórz*wartość.
4. Z listy rozwijanej **Wyświetl klasę danych** wybierz wartość *MovieApp. Movie*.
5. Kliknij przycisk **Dodaj** , aby utworzyć nowy widok.

[![okno dialogowe Nowy projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image12.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image23.png)

**Ilustracja 12**. Dodawanie widoku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image24.png))

Program Visual Studio automatycznie generuje widok na liście 5. Ten widok zawiera formularz HTML, który zawiera pola, które odpowiadają każdej właściwości klasy filmu.

**Lista 5 – Views\Home\Create.aspx**

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample5.aspx)]

> [!NOTE] 
> 
> Formularz HTML wygenerowany przez okno dialogowe Dodaj widok generuje pole identyfikatora formularza. Ponieważ kolumna ID jest kolumną tożsamości, nie jest potrzebne to pole formularza i można ją bezpiecznie usunąć.

Po dodaniu widoku tworzenia można dodać do bazy danych nowe rekordy filmu. Uruchom aplikację, naciskając klawisz F5, a następnie kliknij link Utwórz nowe, aby zobaczyć formularz na rysunku 13. Po zakończeniu i przesłaniu formularza zostanie utworzony nowy rekord bazy danych filmu.

Zwróć uwagę, że automatycznie otrzymujesz walidację formularza. Jeśli nie wprowadzisz daty wydania filmu lub wprowadzisz nieprawidłową datę wydania, formularz zostanie ponownie wyświetlony i pole Data wydania zostanie wyróżnione.

[![okno dialogowe Nowy projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image13.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image25.png)

**Ilustracja 13**. Tworzenie nowego rekordu bazy danych filmów ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image26.png))

## <a name="editing-existing-database-records"></a>Edytowanie istniejących rekordów bazy danych

W poprzednich sekcjach omówiono, jak można wyświetlić i utworzyć nowe rekordy bazy danych. W tej ostatniej sekcji omówiono sposób edytowania istniejących rekordów bazy danych.

Najpierw musimy wygenerować formularz edycji. Ten krok jest prosty, ponieważ program Visual Studio automatycznie generuje formularz edycji dla nas. Otwórz klasę HomeController. vb w edytorze kodu programu Visual Studio i wykonaj następujące kroki:

1. Kliknij prawym przyciskiem myszy metodę Edit () w edytorze kodu i wybierz opcję menu **Dodaj widok** (zobacz rysunek 14).
2. Zaznacz pole wyboru z etykietą **Utwórz widok o jednoznacznie określonym typie**.
3. Z listy rozwijanej **Wyświetl zawartość** wybierz pozycję *Edytowanie*wartości.
4. Z listy rozwijanej **Wyświetl klasę danych** wybierz wartość *MovieApp. Movie*.
5. Kliknij przycisk **Dodaj** , aby utworzyć nowy widok.

Wykonanie tych kroków powoduje dodanie nowego widoku o nazwie Edit. aspx do folderu Views\Home. Ten widok zawiera formularz HTML służący do edytowania rekordu filmu.

[![okno dialogowe Nowy projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image14.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image27.png)

**Ilustracja 14**. Dodawanie widoku edycji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image28.png))

> [!NOTE] 
> 
> Widok edycji zawiera pole formularza HTML, które odpowiada właściwości identyfikatora filmu. Ponieważ nie chcesz, aby użytkownicy edytujący wartość właściwości ID, należy usunąć to pole formularza.

Na koniec należy zmodyfikować kontroler Home, aby obsługiwał edytowanie rekordu bazy danych. Zaktualizowana Klasa HomeController znajduje się na liście 6.

**Lista 6 – Controllers\HomeController.vb (edycja metod)**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample6.vb)]

W liście 6 dodano dodatkową logikę do obu przeciążeń metody Edit (). Pierwsza metoda Edit () zwraca rekord bazy danych filmu, który odnosi się do parametru identyfikatora przesłanego do metody. Drugie Przeciążenie wykonuje aktualizacje rekordu filmu w bazie danych.

Zwróć uwagę, że musisz pobrać oryginalny film, a następnie wywołać ApplyPropertyChanges (), aby zaktualizować istniejący film w bazie danych.

## <a name="summary"></a>Podsumowanie

Celem tego samouczka było zrozumienie środowiska tworzenia aplikacji ASP.NET MVC. Mam nadzieję, że wykryto, że tworzenie aplikacji sieci Web ASP.NET MVC jest bardzo podobne do środowiska tworzenia Active Server stron lub aplikacji ASP.NET.

W tym samouczku zbadamy tylko najbardziej podstawowe funkcje platformy MVC ASP.NET. W przyszłych samouczkach szczegółowe się do tematów, takich jak kontrolery, akcje kontrolera, widoki, wyświetlanie danych i pomocników HTML.

> [!div class="step-by-step"]
> [Wstecz](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs.md)

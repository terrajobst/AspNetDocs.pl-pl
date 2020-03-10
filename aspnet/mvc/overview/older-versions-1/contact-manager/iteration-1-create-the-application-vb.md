---
uid: mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-vb
title: 'Iteracja #1 — Tworzenie aplikacji (VB) | Microsoft Docs'
author: microsoft
description: 'W pierwszej iteracji tworzymy Menedżera kontaktów w najprostszy sposób. Dodaliśmy obsługę podstawowych operacji bazy danych: Tworzenie, odczytywanie, aktualizowanie i D...'
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 5b033582-1646-42c2-b20d-7edc8814e970
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-vb
msc.type: authoredcontent
ms.openlocfilehash: c6bf4712fb734cf14420fd62c9eaf190a2c28168
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78602320"
---
# <a name="iteration-1--create-the-application-vb"></a>Iteracja #1 — Tworzenie aplikacji (VB)

przez [firmę Microsoft](https://github.com/microsoft)

[Pobierz kod](iteration-1-create-the-application-vb/_static/contactmanager_1_vb1.zip)

> W pierwszej iteracji tworzymy Menedżera kontaktów w najprostszy sposób. Dodaliśmy obsługę podstawowych operacji bazy danych: Tworzenie, odczytywanie, aktualizowanie i usuwanie (CRUD).

## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Tworzenie aplikacji Contact Management ASP.NET MVC (VB)

W tej serii samouczków tworzymy całą aplikację do zarządzania kontaktami od początku do końca. Aplikacja Contact Manager umożliwia przechowywanie informacji kontaktowych — nazw, numerów telefonów i adresów e-mail — w celu uzyskania listy osób.

Aplikacja została utworzona przez wiele iteracji. W przypadku każdej iteracji stopniowo ulepszamy aplikację. Celem tej wielu iteracji jest umożliwienie zrozumienia przyczyny każdej zmiany.

- #1 iteracji — Utwórz aplikację. W pierwszej iteracji tworzymy Menedżera kontaktów w najprostszy sposób. Dodaliśmy obsługę podstawowych operacji bazy danych: Tworzenie, odczytywanie, aktualizowanie i usuwanie (CRUD).

- Iteracja #2 — Zwiększ wygląd aplikacji. W tej iteracji ulepszamy wygląd aplikacji, modyfikując domyślną stronę wzorcową widoku MVC ASP.NET i kaskadowy arkusz stylów.

- Iteracja #3 — Dodawanie walidacji formularza. W trzeciej iteracji zostanie dodana podstawowa Walidacja formularza. Uniemożliwiamy użytkownikom przesyłanie formularza bez wykonywania wymaganych pól formularza. Sprawdzamy również adresy e-mail i numery telefonów.

- Iteracja #4 — możliwość swobodnego łączenia aplikacji. W tej czwartej iteracji wykorzystujemy kilka wzorców projektowych oprogramowania, aby ułatwić konserwację i modyfikowanie aplikacji Contact Manager. Na przykład Refaktoryzacja naszej aplikacji używa wzorca repozytorium i wzorca iniekcji zależności.

- #5 iteracji — Utwórz testy jednostkowe. W piątej iteracji upraszczamy obsługę i modyfikację naszej aplikacji przez dodanie testów jednostkowych. Tworzymy klasy modelu danych i kompilujemy testy jednostkowe dla naszych kontrolerów i logiki walidacji.

- Iteracja #6 — Użyj programowania opartego na testach. W tej szóstej iteracji Dodaliśmy nowe funkcje do naszej aplikacji, pisząc testy jednostkowe jako pierwsze i pisząc kod na testach jednostkowych. W tej iteracji dodamy grupy kontaktów.

- Iteracja #7 — Dodawanie funkcji AJAX. W siódmej iteracji poprawimy czas reakcji i wydajność naszej aplikacji przez dodanie obsługi technologii AJAX.

## <a name="this-iteration"></a>Ta iteracja

W pierwszej iteracji tworzymy podstawową aplikację. Celem jest skompilowanie Menedżera kontaktów w najszybszy i najprostszy sposób. W późniejszych iteracjach ulepszamy projektowanie aplikacji.

Aplikacja Contact Manager jest podstawową aplikacją opartą na bazie danych. Za pomocą aplikacji można tworzyć nowe kontakty, edytować istniejące kontakty i usuwać kontakty.

W tej iteracji wykonaj następujące czynności:

1. Aplikacja ASP.NET MVC
2. Tworzenie bazy danych do przechowywania naszych kontaktów
3. Generuj klasę modelu dla naszej bazy danych za pomocą Entity Framework firmy Microsoft
4. Utwórz akcję kontrolera i widok, które umożliwią nam Wyświetlanie listy wszystkich kontaktów w bazie danych
5. Utwórz akcje kontrolera i widok, który umożliwi nam tworzenie nowego kontaktu w bazie danych
6. Twórz akcje kontrolera i widok, który umożliwia nam Edytowanie istniejącego kontaktu w bazie danych
7. Utwórz akcje kontrolera i widok, który umożliwi nam Usuwanie istniejącego kontaktu w bazie danych

## <a name="software-prerequisites"></a>Wymagania wstępne dotyczące oprogramowania

W aplikacjach ASP.NET MVC na komputerze musi być zainstalowany program Visual Studio 2008 lub Visual Web Developer 2008 (Visual Web Developer to bezpłatna wersja programu Visual Studio, która nie zawiera wszystkich zaawansowanych funkcji programu Visual Studio). Możesz pobrać wersję próbną programu Visual Studio 2008 lub Visual Web Developer z następującego adresu:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

> [!NOTE] 
> 
> W przypadku aplikacji ASP.NET MVC z programem Visual Web Developer trzeba mieć zainstalowany program Visual Web Developer Service Pack 1. Bez dodatku Service Pack 1 nie można tworzyć projektów aplikacji sieci Web.

ASP.NET. Strukturę ASP.NET MVC można pobrać z następującego adresu:

[https://www.asp.net/mvc](../../../index.md)

W tym samouczku użyjemy Entity Framework firmy Microsoft w celu uzyskania dostępu do bazy danych. Entity Framework jest dołączona do .NET Framework 3,5 z dodatkiem Service Pack 1. Ten dodatek Service Pack można pobrać z następującej lokalizacji:

[https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;d isplaylang = pl](https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang=en)

Alternatywą dla wykonywania każdego z tych plików jest pobranie jednego z nich, ale można skorzystać z Instalatora platformy sieci Web (Web PI). Możesz pobrać wartość Web PI z następującego adresu:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

## <a name="aspnet-mvc-project"></a>Projekt ASP.NET MVC

Projekt aplikacji sieci Web ASP.NET MVC. Uruchom program Visual Studio i wybierz plik opcji menu **, nowy projekt**. Zostanie wyświetlone okno dialogowe **Nowy projekt** (patrz rysunek 1). Wybierz typ projektu **sieci Web** i szablon **aplikacji sieci Web ASP.NET MVC** . Nazwij nowy projekt *ContactManager* i kliknij przycisk OK.

Upewnij się, że wybrano .NET Framework 3,5 z listy rozwijanej w prawym górnym rogu okna dialogowego **Nowy projekt** . W przeciwnym razie szablon aplikacji sieci Web ASP.NET MVC nie zostanie wyświetlony.

[![okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image1.jpg)](iteration-1-create-the-application-vb/_static/image1.png)

**Ilustracja 01**. okno dialogowe Nowy projekt ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image2.png))

ASP.NET MVC, zostanie wyświetlone okno dialogowe **Tworzenie projektu testu jednostkowego** . Możesz użyć tego okna dialogowego, aby wskazać, że chcesz utworzyć i dodać projekt testu jednostkowego do rozwiązania podczas tworzenia aplikacji ASP.NET MVC. Mimo że testy jednostkowe nie zostaną skompilowane w tej iteracji, należy wybrać opcję **tak, utworzyć projekt testu jednostkowego** , ponieważ planujemy dodać testy jednostkowe w późniejszej iteracji. Dodanie projektu testowego podczas pierwszego tworzenia nowego projektu ASP.NET MVC jest znacznie łatwiejsze niż dodanie projektu testowego po utworzeniu projektu ASP.NET MVC.

> [!NOTE] 
> 
> Ponieważ Visual Web Developer nie obsługuje projektów testowych, nie uzyskujesz okna dialogowego Tworzenie projektu testu jednostkowego podczas korzystania z programu Visual Web Developer.

[![okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image2.jpg)](iteration-1-create-the-application-vb/_static/image3.png)

**Ilustracja 02**. okno dialogowe Tworzenie projektu testu jednostkowego ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image4.png))

Aplikacja ASP.NET MVC pojawia się w oknie Eksplorator rozwiązań programu Visual Studio (patrz rysunek 3). Jeśli nie widzisz okna Eksplorator rozwiązań, możesz otworzyć to okno, wybierając widok opcji menu **, Eksplorator rozwiązań**. Należy zauważyć, że rozwiązanie zawiera dwa projekty: projekt ASP.NET MVC i projekt testowy. Projekt ASP.NET MVC ma nazwę ContactManager, a projekt testowy nosi nazwę ContactManager. tests.

[![okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image3.jpg)](iteration-1-create-the-application-vb/_static/image5.png)

**Ilustracja 03**: okno Eksplorator rozwiązań ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image6.png))

## <a name="deleting-the-project-sample-files"></a>Usuwanie przykładowych plików projektu

Szablon projektu MVC ASP.NET zawiera przykładowe pliki dla kontrolerów i widoków. Przed utworzeniem nowej aplikacji ASP.NET MVC należy usunąć te pliki. Aby usunąć pliki i foldery w oknie Eksplorator rozwiązań, kliknij prawym przyciskiem myszy plik lub folder, a następnie wybierz polecenie menu **Usuń**.

Należy usunąć następujące pliki z projektu ASP.NET MVC:

- \Controllers\HomeController.vb

- \Views\Home\About.aspx

- \Views\Home\Index.aspx

I należy usunąć następujący plik z projektu testowego:

\Controllers\HomeControllerTest.vb

## <a name="creating-the-database"></a>Tworzenie bazy danych

Aplikacja Contact Manager jest aplikacją sieci Web opartą na bazie danych. Korzystamy z bazy danych do przechowywania informacji kontaktowych.

Platforma ASP.NET MVC z dowolnymi nowoczesnymi bazami danych, w tym bazami danych Microsoft SQL Server, Oracle, MySQL i IBM DB2. W tym samouczku używamy bazy danych Microsoft SQL Server. Po zainstalowaniu programu Visual Studio jest dostępna opcja zainstalowania Microsoft SQL Server Express, która jest bezpłatną wersją Microsoft SQL Server Database.

Utwórz nową bazę danych, klikając prawym przyciskiem myszy folder\_danych aplikacji w oknie Eksplorator rozwiązań i wybierając opcję menu **Dodaj, nowy element**. W oknie dialogowym **Dodaj nowy element** wybierz kategorię **dane** i szablon **bazy danych SQL Server** (zobacz rysunek 4). Nazwij nową bazę danych ContactManagerDB. mdf i kliknij przycisk OK.

[![okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image4.jpg)](iteration-1-create-the-application-vb/_static/image7.png)

**Rysunek 04**: Tworzenie nowej bazy danych Microsoft SQL Server Express ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image8.png))

Po utworzeniu nowej bazy danych w oknie Eksplorator rozwiązań zostanie wyświetlona baza danych w folderze dane\_aplikacji. Kliknij dwukrotnie plik ContactManager. mdf, aby otworzyć okno Eksplorator serwera i nawiązać połączenie z bazą danych.

> [!NOTE] 
> 
> Okno Eksplorator serwera jest nazywane oknem Eksplorator bazy danych w przypadku programu Microsoft Visual Web Developer.

Za pomocą okna Eksplorator serwera można tworzyć nowe obiekty bazy danych, takie jak tabele baz danych, widoki, wyzwalacze i procedury składowane. Kliknij prawym przyciskiem myszy folder tabele i wybierz opcję menu **Dodaj nową tabelę**. Zostanie wyświetlony Projektant tabel bazy danych (patrz rysunek 5).

[![okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image5.jpg)](iteration-1-create-the-application-vb/_static/image9.png)

**Ilustracja 05**: Projektant tabel bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image10.png))

Musimy utworzyć tabelę zawierającą następujące kolumny:

<a id="0.2_table01"></a>

| **Nazwa kolumny** | **Typ danych** | **Zezwalaj na wartości null** |
| --- | --- | --- |
| Identyfikator | int | {1&gt;false&lt;1} |
| FirstName | nvarchar (50) | {1&gt;false&lt;1} |
| LastName | nvarchar (50) | {1&gt;false&lt;1} |
| Phone | nvarchar (50) | {1&gt;false&lt;1} |
| Email | nvarchar(255) | {1&gt;false&lt;1} |

Pierwsza kolumna, Identyfikator kolumny, jest specjalna. Należy oznaczyć kolumnę ID jako kolumnę tożsamości i kolumnę klucza podstawowego. Użytkownik wskazuje, że kolumna jest kolumną tożsamości przez rozwijanie właściwości kolumny (patrz dolny rysunek 6) i przewija do właściwości Specyfikacja tożsamości. Ustaw właściwość **(tożsamość)** na wartość **tak**.

Kolumnę należy oznaczyć jako kolumnę klucza podstawowego, zaznaczając ją i klikając przycisk z ikoną klucza. Po oznaczeniu kolumny jako kolumny klucza podstawowego pojawia się ikona klucza obok kolumny (patrz rysunek 6).

Po zakończeniu tworzenia tabeli kliknij przycisk Zapisz (przycisk z ikoną dyskietki), aby zapisać nową tabelę. Nadaj nowej tabeli nazwę *Contacts*.

Po zakończeniu tworzenia tabeli bazy danych kontaktów należy dodać do niej rekordy. Kliknij prawym przyciskiem myszy tabelę kontakty w oknie Eksplorator serwera i wybierz opcję menu **Pokaż dane tabeli**. Wprowadź co najmniej jeden kontakt w wyświetlonej siatce.

## <a name="creating-the-data-model"></a>Tworzenie modelu danych

Aplikacja ASP.NET MVC składa się z modeli, widoków i kontrolerów. Zacznijmy od utworzenia klasy modelu, która reprezentuje tabelę kontaktów utworzoną w poprzedniej sekcji.

W tym samouczku użyjemy Entity Framework firmy Microsoft do automatycznego wygenerowania klasy modelu z bazy danych.

> [!NOTE] 
> 
> Struktura ASP.NET MVC nie jest powiązana z Entity Framework firmy Microsoft w jakikolwiek sposób. Można używać ASP.NET MVC z alternatywnymi technologiami dostępu do baz danych, w tym NHibernate, LINQ to SQL lub ADO.NET.

Wykonaj następujące kroki, aby utworzyć klasy modelu danych:

1. Kliknij prawym przyciskiem myszy folder modele w oknie Eksplorator rozwiązań i wybierz polecenie **Dodaj, nowy element**. Zostanie wyświetlone okno dialogowe **Dodaj nowy element** (zobacz rysunek 6).
2. Wybierz kategorię **dane** i szablon **Entity Data Model ADO.NET** . Nazwij model danych *ContactManagerModel. edmx* i kliknij przycisk **Dodaj** . Zostanie wyświetlony Kreator Entity Data Model (patrz rysunek 7).
3. W kroku **Wybierz zawartość modelu** wybierz pozycję **Generuj z bazy danych** (patrz rysunek 7).
4. W kroku **Wybierz połączenie danych** wybierz bazę danych ContactManagerDB. mdf i wprowadź nazwę *ContactManagerDBEntities* dla ustawień połączenia jednostki (zobacz rysunek 8).
5. W kroku **Wybierz obiekty bazy danych** wybierz pole wyboru z etykietą tabele (zobacz rysunek 9). Model danych będzie zawierać wszystkie tabele zawarte w bazie danych (istnieje tylko jedna tabela kontakty). Wprowadź *modele*przestrzeni nazw. Kliknij przycisk Zakończ, aby zakończyć pracę kreatora.

[![okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image6.jpg)](iteration-1-create-the-application-vb/_static/image11.png)

**Ilustracja 06**. okno dialogowe Dodaj nowy element ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image12.png))

[![okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image7.jpg)](iteration-1-create-the-application-vb/_static/image13.png)

**Ilustracja 07**. Wybierz zawartość modelu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image14.png))

[![okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image8.jpg)](iteration-1-create-the-application-vb/_static/image15.png)

**Ilustracja 08**: Wybieranie połączenia danych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image16.png))

[![okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image9.jpg)](iteration-1-create-the-application-vb/_static/image17.png)

**Ilustracja 09**. Wybierz obiekty bazy danych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image18.png))

Po ukończeniu pracy Kreatora Entity Data Model zostanie wyświetlony Projektant Entity Data Model. Projektant wyświetla klasę, która odpowiada każdej modelowanej tabeli. Powinna zostać wyświetlona jedna klasa o nazwie kontakty.

Kreator Entity Data Model generuje nazwy klas na podstawie nazw tabel bazy danych. Prawie zawsze trzeba zmienić nazwę klasy wygenerowanej przez kreatora. Kliknij prawym przyciskiem myszy klasę kontakty w projektancie, a następnie wybierz opcję menu **Zmień nazwę**. Zmień nazwę klasy z kontaktów (plural) na kontakt (pojedyncza). Po zmianie nazwy klasy Klasa powinna wyglądać jak rysunek 10.

[![okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image10.jpg)](iteration-1-create-the-application-vb/_static/image19.png)

**Ilustracja 10**. Klasa Contact ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image20.png))

W tym momencie utworzyliśmy model bazy danych. Możemy użyć klasy Contact do reprezentowania określonego rekordu kontaktu w bazie danych.

## <a name="creating-the-home-controller"></a>Tworzenie kontrolera macierzystego

Następnym krokiem jest utworzenie naszego kontrolera macierzystego. Kontroler Home jest domyślnym kontrolerem wywoływanym w aplikacji ASP.NET MVC.

Utwórz klasę kontrolera głównego, klikając prawym przyciskiem myszy folder controllers w oknie Eksplorator rozwiązań i wybierając opcję menu **Dodaj, kontroler** (Zobacz Rysunek 11). Zwróć uwagę na pole wyboru zatytułowane **Dodawanie metod akcji dla scenariuszy tworzenia, aktualizacji i szczegółów**. Upewnij się, że to pole wyboru jest zaznaczone przed kliknięciem przycisku **Dodaj** .

[![okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image11.jpg)](iteration-1-create-the-application-vb/_static/image21.png)

**Ilustracja 11**. Dodawanie kontrolera macierzystego ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image22.png))

Podczas tworzenia kontrolera macierzystego pobierana jest Klasa z listy 1.

**Lista 1 — Controllers\HomeController.vb**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample1.vb)]

## <a name="listing-the-contacts"></a>Wyświetlanie listy kontaktów

Aby wyświetlić rekordy w tabeli bazy danych kontaktów, musimy utworzyć akcję indeks () i widok indeksu.

Kontroler Home zawiera już akcję index (). Musimy zmodyfikować tę metodę, aby wyglądała jak lista 2.

**Lista 2 — Controllers\HomeController.vb**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample2.vb)]

Należy zauważyć, że klasa kontrolera macierzystego na liście 2 zawiera pole prywatne o nazwie \_jednostek. Pole jednostki \_reprezentuje jednostki z modelu danych. W celu komunikacji z bazą danych używamy pola \_jednostek.

Metoda index () zwraca widok reprezentujący wszystkie kontakty z tabeli bazy danych kontaktów. Wyrażenie \_jednostek. ContactSet. ToList — () zwraca listę kontaktów jako listę ogólną.

Teraz, gdy utworzymy kontroler indeksu, następnym musimy utworzyć widok indeksu. Przed utworzeniem widoku indeksu Skompiluj swoją aplikację, wybierając opcję menu **kompilacja, Kompiluj rozwiązanie**. Należy zawsze skompilować projekt przed dodaniem widoku, aby lista klas modelu była wyświetlana w oknie dialogowym **Dodawanie widoku** .

Widok indeksu można utworzyć, klikając prawym przyciskiem myszy metodę index () i wybierając opcję menu **Dodaj widok** (patrz rysunek 12). Wybranie tej opcji menu spowoduje otwarcie okna dialogowego **Dodawanie widoku** (patrz rysunek 13).

[![okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image12.jpg)](iteration-1-create-the-application-vb/_static/image23.png)

**Ilustracja 12**. Dodawanie widoku indeksu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image24.png))

W oknie dialogowym **Dodawanie widoku** zaznacz pole wyboru z etykietą **Utwórz widok o jednoznacznie określonym typie**. Wybierz pozycję Wyświetl klasę danych ContactManager. ContactManager i Wyświetl listę zawartości. Wybranie tych opcji spowoduje wygenerowanie widoku zawierającego listę rekordów kontaktów.

[![okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image13.jpg)](iteration-1-create-the-application-vb/_static/image25.png)

**Ilustracja 13**. okno dialogowe Dodawanie widoku ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image26.png))

Po kliknięciu przycisku **Dodaj** zostanie wygenerowany widok indeks z listy 3. Zwróć uwagę na &lt;% @ Page%&gt;, która pojawia się u góry pliku. Widok indeksu dziedziczy z klasy ViewPage&lt;IEnumerable&lt;ContactManager. models. Contact&gt;&gt;. Innymi słowy Klasa modelu w widoku reprezentuje listę jednostek kontaktów.

Treść widoku indeksu zawiera pętlę Foreach, która iteruje przez poszczególne kontakty reprezentowane przez klasę modelu. Wartość każdej właściwości klasy Contact jest wyświetlana w tabeli HTML.

**Lista 3-Views\Home\Index.aspx (niezmodyfikowana)**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample3.aspx)]

Musimy wprowadzić jedną modyfikację w widoku indeksu. Ponieważ nie tworzymy widoku szczegółów, możemy usunąć link szczegóły. Znajdź i usuń następujący kod z widoku indeks:

{. ID = Item. ID})%&gt;

Po zmodyfikowaniu widoku indeksu można uruchomić aplikację Contact Manager. Wybierz opcję menu Debuguj, Rozpocznij debugowanie lub po prostu naciśnij klawisz F5. Przy pierwszym uruchomieniu aplikacji uzyskasz dostęp do okna dialogowego na rysunku 14. Wybierz opcję **Modyfikuj plik Web. config, aby włączyć debugowanie** , a następnie kliknij przycisk OK.

[![okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image14.jpg)](iteration-1-create-the-application-vb/_static/image27.png)

**Ilustracja 14**. Włączanie debugowania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image28.png))

Domyślnie jest zwracany widok indeksu. Ten widok zawiera listę wszystkich danych z tabeli bazy danych kontaktów (zobacz rysunek 15).

[![okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image15.jpg)](iteration-1-create-the-application-vb/_static/image29.png)

**Ilustracja 15**. widok indeksu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image30.png))

Należy zauważyć, że widok indeksu zawiera link z etykietą Utwórz nowe w dolnej części widoku. W następnej sekcji dowiesz się, jak tworzyć nowe kontakty.

## <a name="creating-new-contacts"></a>Tworzenie nowych kontaktów

Aby umożliwić użytkownikom tworzenie nowych kontaktów, należy dodać dwie akcje create () do kontrolera macierzystego. Musimy utworzyć jedną akcję Create (), która zwraca formularz HTML służący do tworzenia nowego kontaktu. Musimy utworzyć drugą akcję Create (), która wykonuje rzeczywiste wstawianie nowego kontaktu do bazy danych.

Nowe metody Create (), które należy dodać do kontrolera macierzystego, znajdują się na liście 4.

**Lista 4-Controllers\HomeController.vb (z metodami Create)**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample4.vb)]

Pierwsza metoda Create () może być wywoływana przy użyciu protokołu HTTP GET, podczas gdy druga metoda Create () może być wywoływana tylko przez POST protokołu HTTP. Innymi słowy, druga metoda Create () może być wywoływana tylko w przypadku ogłaszania formularza HTML. Pierwsza metoda Create () po prostu zwraca widok, który zawiera formularz HTML służący do tworzenia nowego kontaktu. Druga metoda Create () jest znacznie bardziej interesująca: dodaje nowy kontakt do bazy danych.

Należy zauważyć, że druga metoda Create () została zmodyfikowana w celu zaakceptowania wystąpienia klasy Contact. Wartości formularzy ogłoszone w formularzu HTML są powiązane z tą klasą kontaktu przez strukturę ASP.NET MVC automatycznie. Każde pole formularza z formularza Create HTML jest przypisane do właściwości parametru Contact.

Zwróć uwagę, że parametr Contact ma atrybut [bind]. Atrybut [bind] jest używany do wykluczenia właściwości identyfikatora kontaktu z powiązania. Ponieważ Właściwość ID reprezentuje właściwość Identity, nie chcemy ustawić właściwości ID.

W treści metody Create () Entity Framework jest używany do wstawiania nowego kontaktu do bazy danych. Nowy kontakt zostanie dodany do istniejącego zestawu kontaktów i wywoływana jest metoda metody SaveChanges (), aby wypchnąć te zmiany z powrotem do podstawowej bazy danych.

Można wygenerować formularz HTML na potrzeby tworzenia nowych kontaktów, klikając prawym przyciskiem myszy jedną z dwóch metod tworzenia () i wybierając opcję menu **Dodaj widok** (zobacz rysunek 16).

[![okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image16.jpg)](iteration-1-create-the-application-vb/_static/image31.png)

**Ilustracja 16**. Dodawanie widoku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image32.png))

W oknie dialogowym **Dodawanie widoku** wybierz klasę **ContactManager. Contact** i opcję **Utwórz** dla zawartości widoku (Zobacz Rysunek 17). Po kliknięciu przycisku **Dodaj** zostanie automatycznie wygenerowany widok tworzenie.

[![okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image17.jpg)](iteration-1-create-the-application-vb/_static/image33.png)

**Ilustracja 17**. Wyświetlanie rozwinięcia strony ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image34.png))

Widok tworzenie zawiera pola formularza dla każdej właściwości klasy Contact. Kod na potrzeby tworzenia widoku znajduje się na liście 5.

**Lista 5 — Views\Home\Create.aspx**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample5.aspx)]

Po zmodyfikowaniu metod Create () i dodaniu widoku Utwórz można uruchomić aplikację Contact Manager i utworzyć nowe kontakty. Kliknij link **Utwórz nowy** , który pojawia się w widoku indeks, aby przejść do widoku Utwórz. Widok powinien być widoczny na rysunku 18.

[![okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image18.jpg)](iteration-1-create-the-application-vb/_static/image35.png)

**Ilustracja 18**. widok tworzenia ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image36.png))

## <a name="editing-contacts"></a>Edytowanie kontaktów

Dodawanie funkcji do edytowania rekordu kontaktu jest bardzo podobne do dodawania funkcji tworzenia nowych rekordów kontaktów. Najpierw musimy dodać dwie nowe metody edycji do klasy kontrolera macierzystego. Te nowe metody Edit () są zawarte w temacie 6.

**Lista 6-Controllers\HomeController.vb (z metodami edycji)**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample6.vb)]

Pierwsza metoda Edit () jest wywoływana przez operację HTTP GET. Do tej metody jest przesyłany parametr identyfikatora, który reprezentuje identyfikator edytowanego rekordu kontaktu. Entity Framework jest używany do pobrania kontaktu odpowiadającego identyfikatorowi. Zwracany jest widok, który zawiera formularz HTML służący do edytowania rekordu.

Druga metoda Edit () wykonuje rzeczywistą aktualizację bazy danych. Ta metoda akceptuje wystąpienie klasy Contact jako parametr. ASP.NET platformy MVC tworzy automatycznie powiązania pól formularza z formularzem edycji z tą klasą. Należy zauważyć, że podczas edytowania kontaktu nie jest uwzględniany atrybut [bind] (wymagana jest wartość właściwości ID).

Entity Framework jest używany do zapisania zmodyfikowanego kontaktu do bazy danych. Oryginalny kontakt musi zostać najpierw pobrany z bazy danych. Następnie Metoda Entity Framework ApplyPropertyChanges () jest wywoływana, aby zarejestrować zmiany w kontakcie. Na koniec Metoda Entity Framework metody SaveChanges () jest wywoływana w celu utrwalenia zmian w źródłowej bazie danych.

Widok, który zawiera formularz edycji, można wygenerować, klikając prawym przyciskiem myszy metodę Edit () i wybierając opcję menu Dodaj widok. W oknie dialogowym Dodawanie widoku wybierz klasę **ContactManager. models. Contact** i **Edytuj** zawartość widoku (zobacz rysunek 19).

[![okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image19.jpg)](iteration-1-create-the-application-vb/_static/image37.png)

**Ilustracja 19**. Dodawanie widoku edycji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image38.png))

Po kliknięciu przycisku Dodaj zostanie automatycznie wygenerowany nowy widok edycji. Wygenerowany formularz HTML zawiera pola, które odpowiadają każdej właściwości klasy Contact (patrz lista 7).

**Lista 7 — Views\Home\Edit.aspx**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample7.aspx)]

## <a name="deleting-contacts"></a>Usuwanie kontaktów

Jeśli chcesz usunąć kontakty, musisz dodać dwie akcje Delete () do klasy kontrolera macierzystego. Pierwsza akcja Usuń () powoduje wyświetlenie formularza potwierdzenia usuwania. Druga akcja DELETE () wykonuje rzeczywiste usunięcie.

> [!NOTE] 
> 
> Później, w #7 iteracji zmodyfikujemy Menedżera kontaktów, aby obsługiwał jeden krok AJAX Delete.

Dwie nowe metody Delete () są zawarte w liście 8.

**Lista 8-Controllers\HomeController.vb (usuwanie metod)**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample8.vb)]

Pierwsza metoda delete () zwraca formularz potwierdzenia służący do usuwania rekordu kontaktu z bazy danych (zobacz Figure20). Druga metoda delete () wykonuje rzeczywistą operację usuwania względem bazy danych. Po pobraniu oryginalnego kontaktu z bazy danych metody Entity Framework DeleteObject () i metody SaveChanges () są wywoływane w celu przeprowadzenia usuwania bazy danych.

[![okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image20.jpg)](iteration-1-create-the-application-vb/_static/image39.png)

**Ilustracja 20**. widok potwierdzenia usunięcia ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image40.png))

Musimy zmodyfikować widok indeksu, aby zawierał link do usuwania rekordów kontaktów (Zobacz Rysunek 21). Należy dodać następujący kod do tej samej komórki tabeli, która zawiera link Edytuj:

{. ID = Item. ID})%&gt;

[![okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image21.jpg)](iteration-1-create-the-application-vb/_static/image41.png)

**Ilustracja 21**. widok indeksu z linkiem edycji ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image42.png))

Następnie musimy utworzyć widok potwierdzenia usunięcia. Kliknij prawym przyciskiem myszy metodę Delete () w klasie Controller-Home i wybierz opcję menu Dodaj widok. Zostanie wyświetlone okno dialogowe Dodawanie widoku (zobacz rysunek 22).

W przeciwieństwie do przypadku widoków list, Create i Edit, okno dialogowe Dodawanie widoku nie zawiera opcji tworzenia widoku usuwania. Zamiast tego wybierz klasę **ContactManager. models. Contact** Data i **pustą** zawartość widoku. Wybranie opcji pustej zawartości widoku będzie wymagało utworzenia widoku wypróbujemy.

[![okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image22.jpg)](iteration-1-create-the-application-vb/_static/image43.png)

**Ilustracja 22**: Dodawanie widoku potwierdzenia usuwania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image44.png))

Zawartość widoku usuwania znajduje się na liście 9. Ten widok zawiera formularz, który potwierdza, czy określony kontakt powinien zostać usunięty (patrz rysunek 21).

**Lista 9-Views\Home\Delete.aspx**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample9.aspx)]

## <a name="changing-the-name-of-the-default-controller"></a>Zmiana nazwy kontrolera domyślnego

Może bother, że nazwa naszej klasy kontrolera do pracy z kontaktami nosi nazwę klasy HomeController. Czy kontroler nie powinien mieć nazwy ContactController?

Ten problem jest bardzo mały, aby naprawić. Najpierw musimy ponownie nazwa kontrolera macierzystego. Otwórz klasę HomeController w edytorze Visual Studio Code, kliknij prawym przyciskiem myszy nazwę klasy i wybierz opcję menu **Zmień nazwę**. Wybranie tej opcji menu spowoduje otwarcie okna dialogowego Zmień nazwę.

[![okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image23.jpg)](iteration-1-create-the-application-vb/_static/image45.png)

**Ilustracja 23**. Refaktoryzacja nazwy kontrolera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image46.png))

[![okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image24.jpg)](iteration-1-create-the-application-vb/_static/image47.png)

**Ilustracja 24**. Korzystanie z okna dialogowego zmiany nazwy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image48.png))

W przypadku zmiany nazwy klasy kontrolera program Visual Studio zaktualizuje również nazwę folderu w folderze widoki. Program Visual Studio zmieni nazwę folderu \Views\Home na folder \Views\Contact.

Po wprowadzeniu tej zmiany aplikacja nie będzie już miała kontrolera macierzystego. Po uruchomieniu aplikacji uzyskasz stronę błędu na rysunku 25.

[![okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image25.jpg)](iteration-1-create-the-application-vb/_static/image49.png)

**Ilustracja 25**: Brak domyślnego kontrolera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image50.png))

Musimy zaktualizować domyślną trasę w pliku Global. asax, aby można było używać kontrolera Contact zamiast kontrolera macierzystego. Otwórz plik Global. asax i zmodyfikuj domyślny kontroler używany przez trasę domyślną (zobacz Lista 10).

**Lista 10-Global. asax. vb**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample10.vb)]

Po wprowadzeniu tych zmian Menedżer kontaktów zostanie prawidłowo uruchomiony. Teraz zostanie użyta Klasa kontrolera Contact jako kontroler domyślny.

## <a name="summary"></a>Podsumowanie

W tej pierwszej iteracji utworzyliśmy w najszybszy sposób podstawową aplikację do zarządzania kontaktami. Wykorzystałem program Visual Studio, aby automatycznie generować kod początkowy dla naszych kontrolerów i widoków. Wykorzystamy również Entity Framework, aby automatycznie generować klasy modelu bazy danych.

Obecnie możemy wyświetlać, tworzyć, edytować i usuwać rekordy kontaktów w aplikacji Contact Manager. Innymi słowy możemy wykonać wszystkie podstawowe operacje bazy danych wymagane przez aplikację sieci Web opartą na bazie danych.

Niestety, nasza aplikacja zawiera pewne problemy. Najpierw i I niechętnie zezwalają, że aplikacja Contact Manager nie jest najbardziej atrakcyjną aplikacją. Potrzebuje pewnej pracy projektowej. W następnej iteracji dowiesz się, jak można zmienić domyślną stronę wzorcową widoku i kaskadowy arkusz stylów, aby poprawić wygląd aplikacji.

Po drugie nie zaimplementowano żadnej walidacji formularza. Na przykład nie ma nic, aby zapobiec przesłaniu formularza tworzenia kontaktu bez wprowadzania wartości dla żadnego z pól formularza. Ponadto możesz wprowadzić nieprawidłowe numery telefonów i adresy e-mail. Zaczynamy rozwiązywać problemy z walidacją formularza w iteracji #3.

Wreszcie i co najważniejsze, bieżąca iteracja aplikacji Contact Manager nie może być łatwo modyfikowana ani utrzymywana. Na przykład logika dostępu do bazy danych jest rozszerzania bezpośrednio do akcji kontrolera. Oznacza to, że nie możemy zmodyfikować naszego kodu dostępu do danych bez modyfikowania naszych kontrolerów. W kolejnych iteracjach eksplorujemy wzorce projektowe oprogramowania, które możemy zaimplementować, aby umożliwić zmianę odporności Menedżera kontaktów.

> [!div class="step-by-step"]
> [Poprzednie](iteration-7-add-ajax-functionality-cs.md)
> [dalej](iteration-2-make-the-application-look-nice-vb.md)

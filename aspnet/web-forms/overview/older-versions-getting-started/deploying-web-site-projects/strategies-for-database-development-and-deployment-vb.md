---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-vb
title: Strategie tworzenia i wdrażania baz danych (VB) | Microsoft Docs
author: rick-anderson
description: Podczas wdrażania aplikacji opartej na danych po raz pierwszy możesz odślepo skopiować bazę danych w środowisku programistycznym do środowiska produkcyjnego. B...
ms.author: riande
ms.date: 04/23/2009
ms.assetid: 07b8905d-78ac-4252-97fb-8675b3fb0bbf
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-vb
msc.type: authoredcontent
ms.openlocfilehash: 1ea4ade32fb05b9e69647ece7d1a4c400fe9fb21
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74616135"
---
# <a name="strategies-for-database-development-and-deployment-vb"></a>Strategie projektowania i wdrażania baz danych (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz plik PDF](https://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial10_DBDevel_vb.pdf)

> Podczas wdrażania aplikacji opartej na danych po raz pierwszy możesz odślepo skopiować bazę danych w środowisku programistycznym do środowiska produkcyjnego. Jednak wykonanie kopii ślepej w kolejnych wdrożeniach spowoduje zastąpienie wszelkich danych wprowadzonych do produkcyjnej bazy danych. Zamiast tego wdrożenie bazy danych obejmuje zastosowanie zmian wprowadzonych w bazie danych programistycznych od momentu ostatniego wdrożenia w produkcyjnej bazie danych. Ten samouczek analizuje te wyzwania i oferuje różne strategie, które ułatwiają poddawanie i stosowanie zmian wprowadzonych w bazie danych od momentu ostatniego wdrożenia.

## <a name="introduction"></a>Wprowadzenie

Zgodnie z opisem w poprzednich samouczkach wdrożenie aplikacji ASP.NET wiąże się z kopiowaniem odnośnej zawartości ze środowiska deweloperskiego do środowiska produkcyjnego. Wdrożenie nie jest zdarzeniem jednorazowym, ale nie jest wykonywane za każdym razem, gdy następuje wydanie nowej wersji oprogramowania lub zidentyfikowanie i rozwiązanie problemów związanych z bezpieczeństwem. Podczas kopiowania ASP.NET stron, obrazów, plików JavaScript i innych plików do środowiska produkcyjnego nie trzeba się dochodzić do sposobu zmiany tego pliku od momentu ostatniego wdrożenia. Możesz odślepo skopiować plik do środowiska produkcyjnego, zastępując istniejącą zawartość. Niestety, ta prostota nie obejmuje wdrażania bazy danych.

Podczas wdrażania aplikacji opartej na danych po raz pierwszy możesz odślepo skopiować bazę danych w środowisku programistycznym do środowiska produkcyjnego. Jednak wykonanie kopii ślepej w kolejnych wdrożeniach spowoduje zastąpienie wszelkich danych wprowadzonych do produkcyjnej bazy danych. Zamiast tego wdrożenie bazy danych obejmuje zastosowanie *zmian* wprowadzonych w bazie danych programistycznych od momentu ostatniego wdrożenia w produkcyjnej bazie danych. Ten samouczek analizuje te wyzwania i oferuje różne strategie, które ułatwiają poddawanie i stosowanie zmian wprowadzonych w bazie danych od momentu ostatniego wdrożenia.

## <a name="the-challenges-of-deploying-a-database"></a>Wyzwania związane z wdrażaniem bazy danych

Przed wdrożeniem aplikacji opartej na danych po raz pierwszy istnieje tylko jedna baza danych, a mianowicie baza danych w środowisku deweloperskim, na przykład podczas wdrażania aplikacji opartej na danych po raz pierwszy, z której można odślepo Kopiowanie bazy danych w środowisko programistyczne w środowisku produkcyjnym. Jednak po wdrożeniu aplikacji istnieją dwie kopie bazy danych: jedna w programowaniu i jedna w środowisku produkcyjnym.

Między wdrożeniami bazy danych programistycznych i produkcyjnych mogą nie być zsynchronizowane. Schemat produkcyjnej bazy danych s pozostaje niezmieniony, ale schemat bazy danych programistycznych może ulec zmianie po dodaniu nowych funkcji. Możesz dodawać lub usuwać kolumny, tabele, widoki lub procedury składowane. Mogą być również ważne dane, które są dodawane do bazy danych programistycznych. Wiele aplikacji opartych na danych zawiera tabele wyszukiwania wypełnione zakodowanymi danymi specyficznymi dla aplikacji, które nie są edytowane przez użytkownika. Na przykład witryna sieci Web z licytacją może zawierać listę rozwijaną z opcjami opisującymi stan aukcji z licytacją: nowe, takie jak nowy, dobry i uczciwy. Zamiast nakodować twarde te opcje bezpośrednio na liście rozwijanej, zazwyczaj lepiej jest umieścić je w tabeli bazy danych. Jeśli podczas opracowywania, nowy warunek o nazwie Słabe zostanie dodany do tabeli, a następnie podczas wdrażania aplikacji należy dodać ten sam rekord do tabeli odnośników w produkcyjnej bazie danych.

W idealnym przypadku wdrażanie bazy danych będzie wymagało kopiowania bazy danych z projektowania do produkcji. Należy jednak pamiętać, że po wdrożeniu aplikacji i wznowieniu tworzenia produkcyjnej bazy danych są wypełniane rzeczywistymi danymi od rzeczywistych użytkowników. W związku z tym, jeśli po prostu skopiowanie bazy danych z programowania do środowiska produkcyjnego w środowisku produkcyjnym w następnym wdrożeniu zastąpisz produkcyjną bazę danych i utracisz istniejące dane. Wynikiem jest to, że wdrażanie bazy danych jest wrzące w celu zastosowania zmian wprowadzonych w bazie danych programistycznych od momentu ostatniego wdrożenia.

Ze względu na to, że wdrażanie bazy danych obejmuje zastosowanie zmian w schemacie i, prawdopodobnie, danych od ostatniego wdrożenia, należy utrzymać historię zmian (lub określić ją w czasie wdrażania), aby te zmiany mogły być stosowane w środowisku produkcyjnym. Istnieją różne techniki służące do zarządzania i stosowania zmian w modelu danych.

### <a name="defining-the-baseline"></a>Definiowanie linii bazowej

Aby zachować zmiany w bazie danych aplikacji, należy mieć pewien stan początkowy, czyli linię bazową, do której zostaną zastosowane zmiany. W jednym stanie stan początkowy może być pustą bazą danych bez tabel, widoków ani procedur składowanych. Taka linia bazowa skutkuje dużym dziennikiem zmian, ponieważ musi obejmować tworzenie wszystkich tabel, widoków i procedur składowanych bazy danych wraz ze wszystkimi zmianami wprowadzonymi po początkowym wdrożeniu. Na drugim końcu spektrum można ustawić linię bazową jako wersję bazy danych początkowo wdrożoną w środowisku produkcyjnym. Ten wybór skutkuje znacznie mniejszym dziennikiem zmian, ponieważ zawiera on tylko zmiany wprowadzone do bazy danych po pierwszym wdrożeniu. Jest to preferowane podejście. Ponadto możesz wybrać większą część podejścia do drogi, definiując linię bazową jako punkt między początkowym tworzeniem bazy danych a pierwszą wdrożeniem bazy danych.

Po wybraniu linii bazowej Rozważ wygenerowanie skryptu SQL, który można wykonać, aby ponownie utworzyć wersję bazową. Taki skrypt pozwala szybko odtworzyć wersję bazową bazy danych. Ta funkcja jest szczególnie przydatna w większych projektach, w których może istnieć wielu deweloperów pracujących nad projektem lub dodatkowymi środowiskami, takimi jak testowanie lub przemieszczanie, które wymagają własnej kopii bazy danych.

Istnieje wiele narzędzi do dyspozycji do wygenerowania skryptu SQL wersji linii bazowej. W SQL Server Management Studio (SSMS) kliknij prawym przyciskiem myszy bazę danych, przejdź do podmenu zadania, a następnie wybierz opcję Generuj skrypty. Spowoduje to uruchomienie kreatora skryptów, który umożliwia wygenerowanie pliku zawierającego polecenia SQL służące do tworzenia obiektów bazy danych. Kolejną opcją jest Kreator publikacji bazy danych, który może generować polecenia SQL, aby nie tylko utworzyć schemat bazy danych, ale również dane w tabelach bazy danych. Kreator publikowania bazy danych został szczegółowo sprawdzony w samouczku *wdrażanie bazy danych* . Niezależnie od tego, jakiego narzędzia używasz, na końcu powinien znajdować się plik skryptu, którego można użyć do odtworzenia bazowej wersji bazy danych, jeśli zachodzi taka potrzeba.

## <a name="documenting-the-database-changes-in-prose"></a>Dokumentowanie zmian w bazie danych w programie Prose

Najprostszym sposobem, aby zachować dziennik zmian w modelu danych podczas fazy projektowania, jest zapisanie zmian w Prose. Na przykład jeśli podczas opracowywania już wdrożonej aplikacji dodasz nową kolumnę do tabeli `Employees`, usuniesz kolumnę z tabeli `Orders` i dodamy nową tabelę (`ProductCategories`), plik tekstowy lub dokument programu Microsoft Word zostanie zachowany z następującą historią:

<a id="0.8_table01"></a>

| **Zmień datę** | **Szczegóły zmiany** |
| --- | --- |
| 2009-02-03: | Dodano `DepartmentID` kolumny (`int`, a nie NULL) do tabeli `Employees`. Dodano ograniczenie klucza obcego z `Departments.DepartmentID` do `Employees.DepartmentID`. |
| 2009-02-05: | Usunięto `TotalWeight` kolumny z tabeli `Orders`. Dane już przechwycone w skojarzonych rekordach `OrderDetails`. |
| 2009-02-12: | Utworzono tabelę `ProductCategories`. Istnieją trzy kolumny: `ProductCategoryID` (`int`, `IDENTITY`, `NOT NULL`), `CategoryName` (`nvarchar(50)`, `NOT NULL`) i `Active` (`bit`, `NOT NULL`). Dodano ograniczenie PRIMARY KEY do `ProductCategoryID`i wartość domyślną 1, aby `Active`. |

Istnieje szereg wad tego podejścia. W przypadku uruchamiania nie ma żadnej nadzieję, że Automatyzacja. Wszystkie te zmiany należy zastosować do bazy danych, na przykład podczas wdrażania aplikacji — Deweloper musi ręcznie zaimplementować każdą zmianę, pojedynczo. Ponadto, jeśli trzeba odtworzyć określoną wersję bazy danych z linii bazowej przy użyciu dziennika zmian, wykonanie tej operacji zajmie więcej czasu, ponieważ rozmiar dziennika zostanie powiększony. Kolejną wadą tej metody jest to, że przejrzystość i poziom szczegółowości poszczególnych wpisów dziennika zmian jest pozostawiony do osoby rejestrującej zmianę. W zespole z wieloma deweloperami niektóre mogą wprowadzać bardziej szczegółowe, bardziej czytelne lub dokładniejsze wpisy niż inne. Dostępne są również literówki i inne błędy związane z wpisami danych.

Główną zaletą dokumentowania zmian w bazie danych w programie Prose jest prostota. Do tworzenia i modyfikowania obiektów bazy danych nie trzeba znać języka SQL. Zamiast tego można zarejestrować zmiany w Prose i zaimplementować je za pomocą graficznego interfejsu użytkownika SQL Server Management Studio s.

Utrzymywanie dziennika zmian w Prose to, że nie jest bardzo zaawansowane i nie będzie działać poprawnie z określonymi projektami, takimi jak te, które są duże w zakresie, mają częste zmiany w modelu danych lub wymagają wielu deweloperów. Jednak te podejście zadziałało bardzo dobrze w małych projektach z jednym człowiekem, które mają tylko sporadyczne zmiany w modelu danych i w których nie ma silnego tła w składni SQL służącej do tworzenia i modyfikowania obiektów bazy danych.

> [!NOTE]
> Gdy informacje w dzienniku zmian są, technicznie, tylko do czasu wdrożenia, zalecamy zachowanie historii zmian. Jednak zamiast utrzymywać pojedynczy, coraz większy plik dziennika zmian, należy rozważyć posiadanie innego pliku dziennika zmian dla każdej wersji bazy danych. Zwykle podczas wdrażania bazy danych warto ją umieścić w wersji. Utrzymując dziennik zmian, można rozpocząć od linii bazowej ponownie utworzyć dowolną wersję bazy danych, uruchamiając skrypty dziennika zmian od wersji 1 i kontynuując dostęp do wersji wymaganej do ponownego utworzenia.

## <a name="recording-the-sql-change-statements"></a>Rejestrowanie instrukcji zmiany SQL

Główną wadą dla utrzymania dziennika zmian w Prose jest brak automatyzacji. W idealnym przypadku wdrożenie zmian w bazie danych w produkcyjnej bazie danych w czasie wdrażania może być tak proste, jak kliknięcie przycisku do uruchomienia skryptu, a nie ręczne wykonanie listy instrukcji. Taka Automatyzacja jest możliwa przez utrzymywanie dziennika zmian zawierającego te polecenia SQL służące do zmiany modelu danych.

Składnia SQL zawiera kilka instrukcji dotyczących tworzenia i modyfikowania różnych obiektów bazy danych. Na przykład, [*instrukcja CREATE TABLE*](https://msdn.microsoft.com/library/ms174979.aspx), gdy jest wykonywana, tworzy nową tabelę z określonymi kolumnami i ograniczeniami. [*Instrukcja ALTER TABLE*](https://msdn.microsoft.com/library/ms190273.aspx) modyfikuje istniejącą tabelę, dodając, usuwając lub modyfikując jej kolumny lub ograniczenia. Istnieją również instrukcje tworzenia, modyfikowania i upuszczania indeksów, widoków, funkcji zdefiniowanych przez użytkownika, procedur składowanych, wyzwalaczy i innych obiektów bazy danych.

Powracamy do naszego poprzedniego przykładu, obrazu podczas opracowywania już wdrożonej aplikacji Dodaj nową kolumnę do tabeli `Employees`, Usuń kolumnę z tabeli `Orders` i Dodaj nową tabelę (`ProductCategories`). Takie działania spowodują, że plik dziennika zmian z następującymi poleceniami SQL:

[!code-sql[Main](strategies-for-database-development-and-deployment-vb/samples/sample1.sql)]

Wypychanie tych zmian do produkcyjnej bazy danych podczas wdrażania jest operacją jednokrotnego kliknięcia: Otwórz SQL Server Management Studio, Połącz się z produkcyjną bazą danych, Otwórz nowe okno zapytania, wklej zawartość dziennika zmian i kliknij polecenie wykonaj, aby uruchomić skrypt.

## <a name="using-a-comparison-tool-to-synchronize-the-data-models"></a>Synchronizowanie modeli danych przy użyciu narzędzia do porównywania

Dokumentowanie zmian w bazie danych w programie Prose jest łatwe, ale wdrożenie zmian wymaga, aby deweloper wprowadzał każdą zmianę w produkcyjnej bazie danych pojedynczo; dokumentowanie zmian poleceń SQL wprowadza te zmiany w produkcyjnej bazie danych tak proste i szybkie jak kliknięcie przycisku, ale wymaga uczenia i opanowania instrukcji SQL i składni w celu tworzenia i modyfikowania obiektów bazy danych. Narzędzia do porównywania baz danych przyjmują najlepsze z obu metod i odrzucają najgorszą wartość.

Narzędzie do porównywania baz danych porównuje schemat lub dane dwóch baz danych i wyświetla raport podsumowujący pokazujący, jak bazy danych różnią się. Po kliknięciu przycisku można wygenerować polecenia SQL służące do synchronizowania co najmniej jednego obiektu bazy danych. W Nutshell można użyć narzędzia do porównywania baz danych w celu porównania baz danych programistycznych i produkcyjnych w czasie wdrażania, generując plik zawierający polecenia SQL, które po wykonaniu spowodują zastosowanie zmian schematu produkcyjnej bazy danych w celu odzwierciedla schemat programu Development Database s.

Istnieją różne narzędzia do porównywania baz danych innych firm oferowane przez wielu różnych dostawców. Jednym z takich przykładów jest [*porównanie SQL*](http://www.red-gate.com/products/SQL_Compare/)przez [*oprogramowanie czerwonej bramy*](http://www.red-gate.com/). Pozwól, aby zapoznać się z procesem korzystania z funkcji SQL Compare, porównując i zsynchronizuj schematy tworzenia i produkcji baz danych.

> [!NOTE]
> W momencie zapisania aktualnej wersji programu SQL Compare w wersji 7,1, która stanowi $395 koszt wersji Standard. Możesz kontynuować pracę, pobierając bezpłatną 14-dniową wersję próbną.

Po uruchomieniu programu SQL Compare zostanie otwarte okno dialogowe projekty porównawcze zawierające zapisane projekty porównywania SQL. Utwórz nowy projekt. Spowoduje to uruchomienie Kreatora konfiguracji projektu, który wyświetla informacje o bazach danych do porównania (patrz rysunek 1). Wprowadź informacje dotyczące baz danych środowiska programistycznego i produkcyjnego.

[![porównać programistyczne i produkcyjne bazy danych](strategies-for-database-development-and-deployment-vb/_static/image2.jpg)](strategies-for-database-development-and-deployment-vb/_static/image1.jpg)

**Rysunek 1**. Porównanie deweloperskich i produkcyjnych baz danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](strategies-for-database-development-and-deployment-vb/_static/image3.jpg))

> [!NOTE]
> Jeśli baza danych środowiska programistycznego jest plikiem bazy danych programu SQL Express Edition w folderze `App_Data` witryny sieci Web, należy zarejestrować bazę danych na serwerze bazy danych SQL Server Express, aby wybrać ją z okna dialogowego pokazanego na rysunku 1. Najprostszym sposobem na to jest otwarcie SQL Server Management Studio (SSMS), nawiązanie połączenia z serwerem bazy danych SQL Server Express i dołączenie bazy danych. Jeśli na komputerze nie jest zainstalowany program SSMS, możesz pobrać i zainstalować [*wersję bezpłatna SQL Server 2008 Management Studio wersja podstawowa*](https://www.microsoft.com/downloads/details.aspx?FamilyId=7522A683-4CB2-454E-B908-E805E9BD4E28&amp;displaylang=en).

Oprócz wyboru baz danych do porównania można także określić różne ustawienia porównywania na karcie Opcje. Jedną z opcji, którą można włączyć, jest "Ignoruj ograniczenia i nazwy indeksów". Odwołaj się do tego w poprzednim samouczku dodaliśmy obiekty bazy danych usług aplikacji do baz danych programistycznych i produkcyjnych. W przypadku użycia narzędzia `aspnet_regsql.exe` do tworzenia tych obiektów w produkcyjnej bazie danych, należy sprawdzić, czy klucz podstawowy i unikatowe nazwy ograniczeń różnią się między programistyczną a produkcyjnymi bazami danych. W związku z tym porównanie SQL będzie flagować wszystkie tabele usług aplikacji jako różne. Możesz pozostawić niezaznaczone pole "Ignoruj ograniczenia i nazwy indeksów" oraz zsynchronizować nazwy ograniczeń lub poinstruować SQL Compare, aby zignorować te różnice.

Po wybraniu baz danych do porównania (i przejrzeniu opcji porównania) kliknij przycisk Porównaj teraz, aby rozpocząć porównywanie. W ciągu następnych kilku sekund porównanie SQL przeanalizuje schematy dwóch baz danych i wygeneruje raport o tym, jak się różni. Celowo całkowicie wprowadził pewne modyfikacje bazy danych programistycznych, aby pokazać, jak takie rozbieżności są notowane w interfejsie SQL Compare. Jak pokazano na rysunku 2, dodaliśmy kolumnę `BirthDate` do tabeli `Authors`, usunięto kolumnę `ISBN` z tabeli `Books` i dodano nową tabelę, `Ratings`, która ma pozwolić użytkownikom na odwiedzenie stawki przeglądanych ksiąg.

> [!NOTE]
> Zmiany modelu danych wprowadzone w tym samouczku zostały wykonane w celu zilustrowania użycia narzędzia do porównywania baz danych. Te zmiany w bazie danych nie będą znajdować się w kolejnych samouczkach.

[![Compare SQL zawiera różnice między opracowywaniem a produkcyjnymi bazami danych](strategies-for-database-development-and-deployment-vb/_static/image5.jpg)](strategies-for-database-development-and-deployment-vb/_static/image4.jpg)

**Rysunek 2**. Porównanie SQL zawiera różnice między opracowywaniem a produkcyjnymi bazami danych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](strategies-for-database-development-and-deployment-vb/_static/image6.jpg))

Funkcja SQL Compare dzieli obiekty bazy danych na grupy, szybko pokazując, które obiekty istnieją w obu bazach danych, ale różnią się, które obiekty istnieją w jednej bazie danych, ale nie inne, i które obiekty są identyczne. Jak widać, istnieją dwa obiekty, które istnieją w obu bazach danych, ale są różne: tabela `Authors`, do której dodano kolumnę, a tabela `Books`, która została usunięta. Istnieje jeden obiekt znajdujący się tylko w bazie danych programistycznych, a mianowicie nowo utworzona tabela `Ratings`. I istnieją 117 obiektów, które są identyczne w obu bazach danych.

Wybranie obiektu bazy danych powoduje wyświetlenie okna różnice SQL, które pokazuje, w jaki sposób te obiekty różnią się. Okno różnice SQL wyświetlane w dolnej części rysunku 2 oznacza, że tabela `Authors` w bazie danych programistycznych ma kolumnę `BirthDate`, która nie znajduje się w tabeli `Authors` w produkcyjnej bazie danych.

Po przejrzeniu różnic i wybraniu obiektów, które chcesz synchronizować, następnym krokiem jest wygenerowanie poleceń SQL potrzebnych do zaktualizowania schematu produkcyjnej bazy danych w celu dopasowania go do programu Development Database. Jest to realizowane za pomocą Kreatora synchronizacji. Kreator synchronizacji potwierdza, jakie obiekty są synchronizowane i podsumowuje plan akcji (patrz rysunek 3). Możesz natychmiast zsynchronizować bazy danych lub wygenerować skrypt z poleceniami SQL, które mogą być uruchamiane na wypoczynek.

[![użyć Kreatora synchronizacji do zsynchronizowania schematów baz danych](strategies-for-database-development-and-deployment-vb/_static/image8.jpg)](strategies-for-database-development-and-deployment-vb/_static/image7.jpg)

**Rysunek 3**. synchronizowanie schematów baz danych za pomocą Kreatora synchronizacji ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](strategies-for-database-development-and-deployment-vb/_static/image9.jpg))

Narzędzia do porównywania baz danych, takie jak Red bramy oprogramowanie SQL, porównują stosowanie zmian do schematu bazy danych programistycznych w produkcyjnej bazie danych, jak to jest proste i klikanie.

> [!NOTE]
> Porównania SQL porównują i synchronizują dwa *schematy*baz danych. Niestety, nie porównują i nie synchronizują danych w dwóch tabelach baz danych. Oprogramowanie Red Gate oferuje produkt o nazwie [*SQL Data Compare*](http://www.red-gate.com/products/SQL_Data_Compare/) , który porównuje i synchronizuje dane między dwiema bazami danych, ale jest osobnym produktem od porównania z programem SQL i kosztem innego $395.

## <a name="taking-the-application-offline-during-deployment"></a>Przełączanie aplikacji do trybu offline podczas wdrażania

Po zapoznaniu się z tymi samouczkami wdrożenie to proces, który obejmuje wiele czynności: kopiowanie stron ASP.NET, stron wzorcowych, plików CSS, plików JavaScript, obrazów i innych niezbędnych zawartości ze środowiska programistycznego do produkcji naturalne Kopiowanie informacji o konfiguracji specyficznej dla środowiska produkcyjnego, w razie konieczności; i zastosowanie zmian w modelu danych od momentu ostatniego wdrożenia. W zależności od liczby plików i złożoności zmian w bazie danych czynności te można wykonać w dowolnym miejscu od kilku sekund do kilku minut. W tym oknie aplikacja sieci Web jest w strumieniu, a użytkownicy odwiedzający witrynę mogą napotkać błędy lub nieoczekiwane zachowanie.

Podczas wdrażania witryny sieci Web najlepiej przełączyć ją w tryb offline, dopóki wdrożenie nie zostanie ukończone. Przełączenie aplikacji do trybu offline (po zakończeniu procesu wdrażania) jest tak proste jak w przypadku przekazywania pliku, a następnie jego usunięcia. Począwszy od ASP.NET 2,0, sama obecność pliku o nazwie `app_offline.htm` w katalogu głównym aplikacji obejmuje całą witrynę sieci Web "offline". Każde żądanie do strony ASP.NET w tej witrynie jest automatycznie odpowiedziane przy użyciu zawartości pliku `app_offline.htm`. Po usunięciu tego pliku Aplikacja wróci do trybu online.

Przełączenie aplikacji do trybu offline podczas wdrażania, jest tak proste jak przekazywanie pliku `app_offline.htm` do katalogu głównego środowiska produkcyjnego, przed rozpoczęciem procesu wdrażania, a następnie usunięciem go (lub zmiana nazwy na inny) po zakończeniu wdrażania. Aby uzyskać więcej informacji na temat tej techniki, zapoznaj się z artykułem John Peterson s, pobierając [*aplikację ASP.NET w tryb offline*](http://www.15seconds.com/issue/061207.htm).

## <a name="summary"></a>Podsumowanie

Główne wyzwanie związane z wdrażaniem centrów aplikacji opartych na danych na całym świecie wdrażania bazy danych. Ponieważ istnieją dwie wersje bazy danych — jeden w środowisku deweloperskim i jeden w środowisku produkcyjnym — te dwa schematy baz danych mogą stać się niezsynchronizowane po dodaniu nowych funkcji podczas opracowywania. Co więcej, ponieważ produkcyjna baza danych jest wypełniana rzeczywistymi danymi od prawdziwych użytkowników, nie można zastąpić produkcyjnej bazy danych zmodyfikowaną bazą danych programistycznej, taką jak można wdrożyć pliki wchodzące w skład aplikacji (ASP.NET strony, Pliki obrazów itd.). Zamiast tego wdrożenie bazy danych wiąże się z implementacją precyzyjnego zestawu zmian wprowadzonych w bazie danych programistycznych w produkcyjnej bazie danych od momentu ostatniego wdrożenia.

Ten samouczek ogląda trzy techniki do obsługi i stosowania dziennika zmian w bazie danych. Najprostszym podejściem jest zapisanie zmian w Prose. Chociaż ta taktyką wprowadza te zmiany w produkcyjnej bazie danych w procesie ręcznym, nie wymaga znajomości poleceń SQL dotyczących tworzenia i modyfikowania obiektów bazy danych. Bardziej zaawansowaną metodę, która jest znacznie bardziej palatable w większych projektach lub projektach z wieloma deweloperami, polega na zarejestrowaniu zmian w postaci szeregu poleceń SQL. Znacznie hastens te zmiany w docelowej bazie danych. Najlepsze z obu metod można osiągnąć przy użyciu narzędzia do porównywania baz danych, takiego jak porównanie oprogramowania SQL z czerwonym bramą.

Ten samouczek zawiera wprowadzenie do wdrażania aplikacji opartej na danych. Następny zestaw samouczków analizuje, jak odpowiedzieć na błędy w środowisku produkcyjnym. Dowiesz się, jak wyświetlić przyjazną stronę błędów zamiast żółtego ekranu zgonu. Zobaczymy, jak zarejestrować szczegóły błędu s i jak ostrzec o wystąpieniu błędów.

Szczęśliwe programowanie!

> [!div class="step-by-step"]
> [Poprzednie](configuring-a-website-that-uses-application-services-vb.md)
> [dalej](displaying-a-custom-error-page-vb.md)

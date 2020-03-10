---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: Tworzenie bazy danych | Microsoft Docs
author: microsoft
description: Krok 2 przedstawia kroki tworzenia bazy danych przechowującej wszystkie dane dotyczące obiadu i RSVP dla naszej aplikacji NerdDinner.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: b0aa7c8cdf741f44e09ed18e2b2f73fe6bf786ae
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78581005"
---
# <a name="create-a-database"></a>Tworzenie bazy danych

przez [firmę Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Jest to krok 2 bezpłatnego [samouczka dotyczącego aplikacji "NerdDinner"](introducing-the-nerddinner-tutorial.md) , który zawiera instrukcje tworzenia niewielkiej, ale kompletnej aplikacji sieci Web przy użyciu ASP.NET MVC 1.
> 
> Krok 2 przedstawia kroki tworzenia bazy danych przechowującej wszystkie dane dotyczące obiadu i RSVP dla naszej aplikacji NerdDinner.
> 
> Jeśli używasz ASP.NET MVC 3, zalecamy użycie [wprowadzenie ze samouczkami ze sklepu MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .

## <a name="nerddinner-step-2-creating-the-database"></a>NerdDinner krok 2: Tworzenie bazy danych

Będziemy używać bazy danych do przechowywania wszystkich danych obiadu i RSVP dla naszej aplikacji NerdDinner.

Poniższe kroki pokazują, jak utworzyć bazę danych przy użyciu wersji bezpłatna SQL Server Express (którą można łatwo zainstalować przy użyciu wersji 2 [Instalator platformy Microsoft Web](https://www.microsoft.com/web/downloads/platform.aspx)). Cały kod, który piszemy, współdziała z zarówno SQL Server Express, jak i pełnymi SQL Server.

### <a name="creating-a-new-sql-server-express-database"></a>Tworzenie nowej bazy danych SQL Server Express

Zaczniemy od kliknięcia prawym przyciskiem myszy projektu sieci Web, a następnie wybierz polecenie menu **dodaj&gt;nowy element** :

![](create-a-database/_static/image1.png)

Spowoduje to wyświetlenie okna dialogowego "Dodawanie nowego elementu" programu Visual Studio. Będziemy filtrować według kategorii "dane" i wybieramy szablon elementu "SQL Server Database":

![](create-a-database/_static/image2.png)

Nazwamy SQL Server Express bazą danych, w której chcemy utworzyć "NerdDinner. mdf" i nacisnąć przycisk OK. Program Visual Studio będzie następnie pytał nas, jeśli chcemy dodać ten plik do naszego katalogu danych \app\_(który jest już skonfigurowany do odczytu i zapisu na listach ACL zabezpieczeń):

![](create-a-database/_static/image3.png)

Po kliknięciu przycisku "tak" zostanie utworzona nowa baza danych i zostanie ona dodana do naszego Eksplorator rozwiązań:

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a>Tworzenie tabel w bazie danych

Mamy teraz nową pustą bazę danych. Dodajmy do niego tabele.

W tym celu przejdziemy do okna karty "Eksplorator serwera" w programie Visual Studio, co umożliwi nam zarządzanie bazami danych i serwerami. SQL Server Express bazy danych przechowywane w folderze dane\_\app naszej aplikacji będą automatycznie wyświetlane w Eksplorator serwera. Opcjonalnie można użyć ikony "Połącz z bazą danych" w górnej części okna "Eksplorator serwera", aby dodać do listy dodatkowe SQL Server bazy danych (lokalne i zdalne):

![](create-a-database/_static/image5.png)

Dodamy dwie tabele do naszej bazy danych NerdDinner — jedną do przechowywania naszych obiadów, a druga do śledzenia akceptacji RSVP do nich. Aby utworzyć nowe tabele, kliknij prawym przyciskiem myszy folder "tabele" w naszej bazie danych i wybierz polecenie menu "Dodaj nową tabelę":

![](create-a-database/_static/image6.png)

Spowoduje to otwarcie projektanta tabel, który pozwala skonfigurować schemat naszej tabeli. W naszej tabeli "obiady" dodamy 10 kolumn danych:

![](create-a-database/_static/image7.png)

Chcemy, aby kolumna "DinnerID" była unikatowym kluczem podstawowym dla tabeli. Można to skonfigurować, klikając prawym przyciskiem myszy kolumnę "DinnerID" i wybierając pozycję "Ustaw klucz podstawowy":

![](create-a-database/_static/image8.png)

Oprócz tworzenia DinnerID klucza podstawowego należy również skonfigurować go jako kolumnę "Identity", której wartość jest automatycznie zwiększana w miarę dodawania nowych wierszy danych do tabeli (oznacza to, że pierwszy wstawiony wiersz obiadu będzie miał DinnerID 1, drugi wstawiony wiersz będzie miał DinnerID 2 itd.

Możemy to zrobić, wybierając kolumnę "DinnerID", a następnie użyć edytora "Właściwości kolumny", aby ustawić właściwość "(is Identity)" dla kolumny na wartość "yes". Będziemy używać standardowych ustawień domyślnych tożsamości (Zacznij od 1 i przyrost 1 w każdym nowym wierszu obiadu):

![](create-a-database/_static/image9.png)

Następnie zapiszemy naszą tabelę, wpisując CTRL-S lub przy użyciu polecenia menu **zapisz&gt;pliku** . Spowoduje to wyświetlenie monitu o podanie nazwy tabeli. Nazwamy "obiad":

![](create-a-database/_static/image10.png)

Nasza nowa tabela obiadów zostanie następnie wyświetlona w bazie danych w Eksploratorze serwera.

Następnie powtórzę powyższe kroki i utworzysz tabelę "RSVP". Ta tabela zawiera 3 kolumny. Skonfigurujemy kolumnę RsvpID jako klucz podstawowy, a także ustawimy ją jako kolumnę tożsamości:

![](create-a-database/_static/image11.png)

Zapiszemy ją i nadaj jej nazwę "RSVP".

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a>Konfigurowanie relacji klucza obcego między tabelami

Mamy teraz dwie tabele w bazie danych. Nasz ostatni krok projektowy schematu będzie w stanie skonfigurować relację "jeden do wielu" między tymi dwiema tabelami, dzięki czemu możemy skojarzyć każdy wiersz obiadu z zerowymi lub większymi wierszami RSVP, które mają zastosowanie do tego. Możemy to zrobić przez skonfigurowanie kolumny "DinnerID" w tabeli protokołu RSVP w taki sposób, aby zawierała ona relację klucza obcego z kolumną "DinnerID" w tabeli "obiady".

W tym celu otworzymy tabelę protokołu RSVP w Projektancie tabel, klikając ją dwukrotnie w Eksploratorze serwera. Następnie wybierz kolumnę "DinnerID" w tym miejscu, kliknij prawym przyciskiem myszy i wybierz "relacje..." polecenie menu kontekstowego:

![](create-a-database/_static/image12.png)

Spowoduje to wyświetlenie okna dialogowego, za pomocą którego można skonfigurować relacje między tabelami:

![](create-a-database/_static/image13.png)

Kliknij przycisk "Dodaj", aby dodać nową relację do okna dialogowego. Po dodaniu relacji rozwiniesz węzeł widoku drzewa "tabele i kolumny" w siatce właściwości z prawej strony okna dialogowego, a następnie kliknij przycisk "...". po prawej stronie:

![](create-a-database/_static/image14.png)

Kliknięcie "..." zostanie wyświetlone inne okno dialogowe, które pozwala określić, które tabele i kolumny są uwzględnione w relacji, a także zezwolić nam na nazwę relacji.

Zmienimy tabelę klucza podstawowego na obiady, a następnie wybierz kolumnę "DinnerID" w tabeli obiady jako klucz podstawowy. Nasza tabela protokołu RSVP będzie tabelą kluczy obcych i RSVP. Kolumna DinnerID zostanie skojarzona jako klucz obcy:

![](create-a-database/_static/image15.png)

Teraz każdy wiersz w tabeli RSVP zostanie skojarzony z wierszem w tabeli obiadu. SQL Server będzie zachować integralność referencyjną dla nas — i uniemożliwić nam Dodawanie nowego wiersza RSVP, jeśli nie wskazuje na prawidłowy wiersz obiadu. Zapobiega to również usunięciu wiersza obiadu, jeśli nadal istnieją wiersze RSVP.

### <a name="adding-data-to-our-tables"></a>Dodawanie danych do naszych tabel

Zakończmy Dodawanie przykładowych danych do naszej tabeli obiadów. Aby dodać dane do tabeli, kliknij ją prawym przyciskiem myszy w Eksplorator serwera i wybierz polecenie "Pokaż dane tabeli":

![](create-a-database/_static/image16.png)

Dodamy kilka wierszy danych obiadu, których możemy użyć później, podczas wdrażania aplikacji:

![](create-a-database/_static/image17.png)

### <a name="next-step"></a>Następny krok

Zakończono Tworzenie bazy danych. Teraz Utwórz klasy modelu, za pomocą których możemy wysyłać zapytania i aktualizować je.

> [!div class="step-by-step"]
> [Poprzednie](create-a-new-aspnet-mvc-project.md)
> [dalej](build-a-model-with-business-rule-validations.md)

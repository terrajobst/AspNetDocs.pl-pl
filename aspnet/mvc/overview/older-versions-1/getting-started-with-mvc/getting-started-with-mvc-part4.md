---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: Tworzenie bazy danych | Microsoft Docs
author: shanselman
description: Jest to samouczek początkującego, który wprowadza podstawy ASP.NET MVC. Utwórz prostą aplikację sieci Web, która odczytuje i zapisuje dane w bazie danych.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: 995db714ce6365415d06dc458aee84a31c7c8fd6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78581439"
---
# <a name="creating-a-database"></a>Tworzenie bazy danych

przez [Scott Hanselman](https://github.com/shanselman)

> Jest to samouczek początkującego, który wprowadza podstawy ASP.NET MVC. Utworzysz prostą aplikację sieci Web, która odczytuje i zapisuje dane w bazie danych. Odwiedź [centrum learning ASP.NET MVC](../../../index.md) , aby znaleźć inne samouczki i przykłady MVC ASP.NET.

W tej sekcji zamierzamy utworzyć nową bazę danych SQL Express, która będzie używana do przechowywania i pobierania danych filmów. W środowisku IDE programu Visual Web Developer wybierz pozycję Widok | Eksplorator serwera. Kliknij prawym przyciskiem myszy pozycję połączenia danych, a następnie kliknij pozycję Dodaj połączenie.

![Addconnection](getting-started-with-mvc-part4/_static/image1.png)

W oknie dialogowym Wybierz źródło danych wybierz pozycję Microsoft SQL Server i wybierz pozycję Kontynuuj.

![](getting-started-with-mvc-part4/_static/image2.png)

W oknie dialogowym Dodawanie połączenia wprowadź wartość ".\SQLEXPRESS" dla nazwy serwera, a następnie wprowadź "filmy" jako nazwę nowej bazy danych.

[okno dialogowe Dodawanie połączenia ![](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)

Kliknij przycisk OK. pojawi się monit z prośbą o utworzenie tej bazy danych. Wybierz pozycję tak.

[![tworzyć filmy?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)

Teraz masz pustą bazę danych w Eksplorator serwera.

![Dodaj nową tabelę](getting-started-with-mvc-part4/_static/image7.png)

Kliknij prawym przyciskiem myszy tabele i kliknij polecenie Dodaj tabelę. Zostanie wyświetlony Projektant tabel. Dodaj kolumny dla identyfikatora, tytułu, ReleaseDate, gatunku i ceny. Kliknij prawym przyciskiem myszy kolumnę ID, a następnie kliknij pozycję Ustaw klucz podstawowy. Oto, jak wyglądają moje obszary projektowania.

[Edytor tabeli bazy danych ![](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)

Ponadto wybierz kolumnę ID, a następnie w obszarze właściwości kolumny poniżej Zmień wartość "Specyfikacja tożsamości" na "tak".

[![właściwości IsIdentity-Column](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)

Gdy wszystko będzie gotowe, kliknij ikonę Zapisz na pasku narzędzi lub wybierz pozycję plik | Zapisz z menu i nazwij tabelę "**Movie**" (pojedynczo). Mamy bazę danych i tabelę.

[![wybierz nazwę](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)

Wróć do Eksplorator serwera i kliknij prawym przyciskiem myszy tabelę filmu, a następnie wybierz polecenie "Pokaż dane tabeli". Wprowadź kilka filmów, aby nasze bazy danych miały pewne dane.

[Edytowanie tabeli bazy danych ![](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)

## <a name="creating-a-model"></a>Tworzenie modelu

Teraz przełącz się z powrotem do Eksplorator rozwiązań po prawej stronie IDE, a następnie kliknij prawym przyciskiem myszy folder modele i wybierz polecenie Dodaj | Nowy element.

[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)

Utworzymy model jednostki na podstawie naszej nowej bazy danych. Spowoduje to dodanie zestawu klas do naszego projektu, który ułatwia wykonywanie zapytań i manipulowanie danymi w bazie danych. Wybierz węzeł dane po lewej stronie okna dialogowego, a następnie wybierz szablon elementu ADO.NET Entity Data Model. Nadaj mu nazwę filmy. edmx.

[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)

Kliknij przycisk "Dodaj". Następnie zostanie uruchomiony Kreator Entity Data Model.

W nowym oknie dialogowym, które się pojawi, wybierz pozycję Generuj z bazy danych. Ponieważ właśnie została utworzona baza danych, będziemy musieli powiedzieć Entity Framework o naszej nowej bazie danych i jej tabeli. Kliknij przycisk Dalej, aby zapisać połączenie z bazą danych w konfiguracji aplikacji sieci Web. Zaznacz pole wyboru tabele i film, a następnie kliknij przycisk Zakończ.

[Kreator Entity Data Model ![](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)

Teraz możemy zobaczyć nową tabelę filmów w Entity Framework Designer i uzyskać do niej dostęp z kodu.

[![filmów — Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)

Na powierzchni projektowej można zobaczyć "film". Ta klasa jest mapowana na tabelę "film" w naszej bazie danych, a każda właściwość w niej jest mapowana na kolumnę z tabelą. Każde wystąpienie klasy "Movie" będzie odpowiadać wierszowi w tabeli "Movie".

Jeśli nie chcesz używać domyślnych konwencji nazewnictwa i mapowania używanych przez Entity Framework, możesz użyć projektanta Entity Framework, aby zmienić lub dostosować je. W przypadku tej aplikacji będziemy używać ustawień domyślnych i po prostu zapiszą plik w stanie takim, w jakim się znajdują.

Teraz pracujemy nad niektórymi rzeczywistymi danymi!

> [!div class="step-by-step"]
> [Poprzednie](getting-started-with-mvc-part3.md)
> [dalej](getting-started-with-mvc-part5.md)

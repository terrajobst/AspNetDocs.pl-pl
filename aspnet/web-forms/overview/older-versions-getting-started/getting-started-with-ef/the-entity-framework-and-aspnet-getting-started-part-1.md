---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
title: Wprowadzenie z Entity Framework 4,0 Database First i ASP.NET 4 formularzy sieci Web | Microsoft Docs
author: tdykstra
description: Przykładowa aplikacja internetowa Contoso University pokazuje, jak tworzyć aplikacje ASP.NET Web Forms przy użyciu Entity Framework 4,0 i programu Visual Studio 2010...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: 5cb00916-8f46-491f-be25-4739a615d619
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
msc.type: authoredcontent
ms.openlocfilehash: fd88b32ad2a65e5b4c7ead15f0d6dc5dc6e97e75
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78630460"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms"></a>Wprowadzenie z formularzami Entity Framework 4,0 Database First i ASP.NET 4

Autor [Dykstra](https://github.com/tdykstra)

> Przykładowa aplikacja internetowa Contoso University pokazuje, jak tworzyć aplikacje ASP.NET Web Forms przy użyciu Entity Framework 4,0 i programu Visual Studio 2010. Przykładowa aplikacja jest witryną internetową fikcyjnej firmy Contoso University. Obejmuje funkcje, takie jak czasowej dla uczniów, tworzenia kurs i przypisania instruktora.
> 
> W tym samouczku przedstawiono C#przykłady w temacie. [Przykład do pobrania](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) zawiera kod w obu C# i Visual Basic.
> 
> ## <a name="database-first"></a>Database First
> 
> Istnieją trzy sposoby pracy z danymi w Entity Framework: *Database First*, *model First*i *Code First*. Ten samouczek jest przeznaczony dla Database First. Aby uzyskać informacje o różnicach między tymi przepływami pracy i wskazówkami dotyczącymi sposobu wybierania najlepszego z nich dla danego scenariusza, zobacz [Entity Frameworke przepływy pracy projektowania](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>Formularze sieci Web
> 
> Ta seria samouczków korzysta z modelu ASP.NET Web Forms i zakłada, że wiesz, jak korzystać z formularzy sieci Web ASP.NET w programie Visual Studio. Jeśli nie, zobacz [wprowadzenie z formularzami sieci Web ASP.NET 4,5](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Jeśli wolisz pracować z platformą MVC ASP.NET, zobacz [wprowadzenie z Entity Framework przy użyciu ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="software-versions"></a>Wersje oprogramowania
> 
> | **Pokazane w samouczku** | **Współpracuje również z** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Visual Studio 2010 Express for Web. Samouczek nie został przetestowany z nowszymi wersjami programu Visual Studio. Istnieje wiele różnic w wyborze menu, oknach dialogowych i szablonach. |
> | .NET 4 | Program .NET 4,5 jest zgodny z platformą .NET 4, ale samouczek nie został przetestowany z platformą .NET 4,5. |
> | Entity Framework 4 | Samouczek nie został przetestowany z nowszymi wersjami Entity Framework. Począwszy od Entity Framework 5, EF używa domyślnie `DbContext API`, które zostały wprowadzone z EF 4,1. Formant EntityDataSource został zaprojektowany tak, aby korzystał z interfejsu API `ObjectContext`. Aby uzyskać informacje o sposobach używania kontrolki EntityDataSource z interfejsem API `DbContext`, zobacz [ten wpis w blogu](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx). |
> 
> ## <a name="questions"></a>Masz
> 
> Jeśli masz pytania, które nie są bezpośrednio związane z samouczkiem, możesz je ogłosić na [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), [forum Entity Framework i LINQ to Entities](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/)lub [StackOverflow.com](http://stackoverflow.com/).

## <a name="overview"></a>Omówienie

Aplikacja, którą tworzysz w tych samouczkach, to prosta witryna sieci Web University.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image1.png)

Użytkownicy mogą przeglądać i aktualizacji dla uczniów, kursu i informacji przez instruktorów. Poniżej przedstawiono kilka ekranów, które zostaną utworzone.

[![Image30](the-entity-framework-and-aspnet-getting-started-part-1/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image3.png)

[![Image37](the-entity-framework-and-aspnet-getting-started-part-1/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image5.png)

[![Image31](the-entity-framework-and-aspnet-getting-started-part-1/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image7.png)

[![Image32](the-entity-framework-and-aspnet-getting-started-part-1/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image9.png)

## <a name="creating-the-web-application"></a>Tworzenie aplikacji sieci Web

Aby rozpocząć pracę z samouczkiem, Otwórz program Visual Studio, a następnie utwórz nowy projekt aplikacji sieci Web ASP.NET za pomocą szablonu **aplikacji sieci web ASP.NET** :

[![Image01](the-entity-framework-and-aspnet-getting-started-part-1/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image11.png)

Ten szablon służy do tworzenia projektu aplikacji sieci Web, który zawiera już arkusz stylów i strony wzorcowe:

[![Image02](the-entity-framework-and-aspnet-getting-started-part-1/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image13.png)

Otwórz plik *site. Master* i zmień wartość "My ASP.NET Application" na "Uniwersytet firmy Contoso".

[!code-html[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample1.html)]

Znajdź formant *menu* o nazwie `NavigationMenu` i zastąp go następującym znacznikiem, co spowoduje dodanie elementów menu dla tworzonych stron.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample2.aspx)]

Otwórz stronę *default. aspx* i zmień formant `Content` o nazwie `BodyContent` na this:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample3.aspx)]

Masz już prostą stronę główną z linkami do różnych stron, które będą tworzone:

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image15.png)

## <a name="creating-the-database"></a>Tworzenie bazy danych

Na potrzeby tych samouczków użyjesz Entity Framework projektanta modelu danych, aby automatycznie utworzyć model danych na podstawie istniejącej bazy danych (często nazywanej pierwszym podejściem *bazy danych* ). Alternatywą, która nie jest pokryty w tej serii samouczków, jest ręczne utworzenie modelu danych, a następnie wygenerowanie przez projektanta skryptów tworzących bazę danych (podejście *najpierw modelu* ).

W przypadku metody pierwszej bazy danych używanej w tym samouczku następnym krokiem jest dodanie bazy danych do lokacji. Najprostszym sposobem jest pobranie projektu, który jest dostępny w tym samouczku. Następnie kliknij prawym przyciskiem myszy folder *\_danych aplikacji* , wybierz pozycję **Dodaj istniejący element**i wybierz plik bazy danych *szkoły. mdf* ze pobranego projektu.

Alternatywą jest wykonanie instrukcji dotyczących [tworzenia przykładowej bazy danych szkoły](https://msdn.microsoft.com/library/bb399731.aspx). Niezależnie od tego, czy pobierasz bazę danych, czy tworzysz ją, skopiuj plik *szkoły. mdf* z następującego folderu do aplikacji aplikacji *\_* :

`%PROGRAMFILES%\Microsoft SQL Server\MSSQL10.SQLEXPRESS\MSSQL\DATA`

(W tej lokalizacji pliku *MDF* przyjęto założenie, że używasz SQL Server 2008 Express).

Jeśli utworzysz bazę danych ze skryptu, wykonaj następujące kroki, aby utworzyć diagram bazy danych:

1. W **Eksplorator serwera**rozwiń węzeł **połączenia danych**, rozwiń węzeł *szkoła. mdf*, kliknij prawym przyciskiem myszy **diagramy bazy danych**i wybierz polecenie **Dodaj nowy diagram**.

    [![Image35](the-entity-framework-and-aspnet-getting-started-part-1/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image17.png)
2. Zaznacz wszystkie tabele, a następnie kliknij przycisk **Dodaj**.

    [![Image36](the-entity-framework-and-aspnet-getting-started-part-1/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image19.png)

    SQL Server tworzy diagram bazy danych, w którym znajdują się tabele, kolumny w tabelach i relacje między tabelami. Możesz przenieść tabele, aby uporządkować je w dowolny sposób.
3. Zapisz diagram jako "SchoolDiagram" i zamknij go.

Jeśli pobrano plik *. mdf* z tym samouczkiem, można wyświetlić diagram bazy danych, klikając dwukrotnie pozycję **SchoolDiagram** w obszarze **diagramy bazy danych** w **Eksplorator serwera**.

[![Image38](the-entity-framework-and-aspnet-getting-started-part-1/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image21.png)

Diagram wygląda podobnie do tego (tabele mogą znajdować się w różnych lokalizacjach, od przedstawionych tutaj):

[![Image04](the-entity-framework-and-aspnet-getting-started-part-1/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image23.png)

## <a name="creating-the-entity-framework-data-model"></a>Tworzenie modelu danych Entity Framework

Teraz można utworzyć model danych Entity Framework z tej bazy danych. Model danych można utworzyć w folderze głównym aplikacji, ale w tym samouczku należy umieścić go w folderze o nazwie *dal* (na potrzeby warstwy dostępu do danych).

W **Eksplorator rozwiązań**Dodaj folder projektu o nazwie *dal* (Upewnij się, że znajduje się on w projekcie, a nie w ramach rozwiązania).

Kliknij prawym przyciskiem myszy folder *dal* , a następnie wybierz pozycję **Dodaj** i **nowy element**. W obszarze **zainstalowane szablony**wybierz pozycję **dane**, wybierz szablon **Entity Data Model ADO.NET** , nadaj mu nazwę *SchoolModel. edmx*, a następnie kliknij przycisk **Dodaj**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-1/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image25.png)

Spowoduje to uruchomienie Kreatora Entity Data Model. W pierwszym kroku kreatora opcja **Generuj z bazy danych** jest domyślnie zaznaczona. Kliknij przycisk **Dalej**.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-1/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image27.png)

W kroku **Wybierz połączenie danych** pozostaw wartości domyślne i kliknij przycisk **dalej**. Baza danych szkoły jest domyślnie zaznaczona, a ustawienie połączenia jest zapisywane w pliku *Web. config* jako **SchoolEntities**.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-1/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image29.png)

W kroku kreatora **Wybierz obiekty bazy danych** zaznacz wszystkie tabele z wyjątkiem `sysdiagrams` (które zostały utworzone dla utworzonego wcześniej diagramu), a następnie kliknij przycisk **Zakończ**.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-1/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image31.png)

Po zakończeniu tworzenia modelu program Visual Studio Wyświetla graficzną reprezentację Entity Framework obiektów (jednostek), które odpowiadają tabelom bazy danych. (Podobnie jak w przypadku diagramu bazy danych lokalizacja poszczególnych elementów może różnić się od tego, co widzisz na tej ilustracji. Możesz przeciągnąć elementy, aby dopasować je do ilustracji, jeśli chcesz.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-1/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image33.png)

## <a name="exploring-the-entity-framework-data-model"></a>Eksplorowanie modelu danych Entity Framework

Można zobaczyć, że diagram jednostki wygląda bardzo podobnie do diagramu bazy danych z kilkoma różnicami. Jedną z różnic jest dodanie symboli na końcu każdego skojarzenia, które wskazują typ skojarzenia (relacje między tabelami są nazywane skojarzeniami jednostek w modelu danych):

- Skojarzenie "jeden do zera" jest reprezentowane przez "1" i "0.. 1".

    [![Image39](the-entity-framework-and-aspnet-getting-started-part-1/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image35.png)

    W takim przypadku jednostka `Person` może być skojarzona z jednostką `OfficeAssignment` lub nie. Jednostka `OfficeAssignment` musi być skojarzona z jednostką `Person`. Innymi słowy, instruktor może lub nie może być przypisany do biura, a każdy pakiet może być przypisany tylko do jednego instruktora.
- Skojarzenie "jeden do wielu" jest reprezentowane przez "1" i "\*".

    [![Image40](the-entity-framework-and-aspnet-getting-started-part-1/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image37.png)

    W takim przypadku jednostka `Person` może lub nie ma skojarzonych `StudentGrade` jednostek. Jednostka `StudentGrade` musi być skojarzona z jedną jednostką `Person`. jednostki `StudentGrade` w rzeczywistości reprezentują zarejestrowane kursy w tej bazie danych. Jeśli student jest zarejestrowany w kursie i nie ma jeszcze żadnej klasy, właściwość `Grade` ma wartość null. Innymi słowy, Student nie może zostać zarejestrowany w żadnym kursie, może zostać zarejestrowany w jednym kursie lub być zarejestrowany w wielu kursach. Każda Klasa w zarejestrowanym kursie ma zastosowanie tylko do jednego ucznia.
- Skojarzenie wiele-do-wielu jest reprezentowane przez "\*" i "\*".

    [![Image41](the-entity-framework-and-aspnet-getting-started-part-1/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image39.png)

    W takim przypadku jednostka `Person` może lub nie ma skojarzonych `Course` jednostek, a odwrócenie również jest prawdziwe: jednostka `Course` może lub nie mieć skojarzonych jednostek `Person`. Innymi słowy, instruktor może uczyć się wielu kursów, a kurs może być nauczany przez wielu instruktorów. (W tej bazie danych ta relacja ma zastosowanie tylko do instruktorów; nie łączy uczniów z kursami. Studenci są połączeni z kursami według tabeli StudentGrades).

Kolejną różnicą między diagramem bazy danych i modelem danych jest dodatkowa sekcja **właściwości nawigacji** dla każdej jednostki. Właściwość nawigacji jednostki odwołuje się do powiązanych jednostek. Na przykład właściwość `Courses` w jednostce `Person` zawiera kolekcję wszystkich `Course` jednostek, które są powiązane z tą jednostką `Person`.

[![Image12](the-entity-framework-and-aspnet-getting-started-part-1/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image41.png)

Jeszcze inna różnica między bazą danych a modelem danych to brak `CourseInstructor`j tabeli skojarzenia, która jest używana w bazie danych do łączenia `Person` i `Course` tabel w relacji wiele-do-wielu. Właściwości nawigacji umożliwiają pobieranie powiązanych jednostek `Course` z jednostki `Person` i powiązanych jednostek `Person` z jednostki `Course`, więc nie ma potrzeby reprezentowania tabeli skojarzenia w modelu danych.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-1/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image43.png)

Na potrzeby tego samouczka Załóżmy, że kolumna `FirstName` tabeli `Person` rzeczywiście zawiera imię i nazwisko osoby. Należy zmienić nazwę pola, aby to odzwierciedlić, ale administrator bazy danych może nie chcieć zmieniać bazy danych. Można zmienić nazwę właściwości `FirstName` w modelu danych, pozostawiając jej odpowiednik niezmieniony.

W projektancie kliknij prawym przyciskiem myszy pozycję **FirstName** w jednostce `Person`, a następnie wybierz polecenie **Zmień nazwę**.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-1/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image45.png)

Wpisz nową nazwę "FirstMidName". To zmienia sposób odwoływania się do kolumny w kodzie bez zmiany bazy danych.

[![Image29](the-entity-framework-and-aspnet-getting-started-part-1/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image47.png)

Przeglądarka modelu zapewnia inny sposób wyświetlania struktury bazy danych, struktury modelu danych i mapowania między nimi. Aby je wyświetlić, kliknij prawym przyciskiem myszy pusty obszar w Projektancie jednostki, a następnie kliknij pozycję **przeglądarka modeli**.

[![Image18](the-entity-framework-and-aspnet-getting-started-part-1/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image49.png)

W okienku **przeglądarka modeli** zostanie wyświetlony widok drzewa. (Okienko **przeglądarki modelu** może być zadokowane w okienku **Eksplorator rozwiązań** ). Węzeł **SchoolModel** reprezentuje strukturę modelu danych, a węzeł **SchoolModel. Store** reprezentuje strukturę bazy danych.

[![Image26](the-entity-framework-and-aspnet-getting-started-part-1/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image51.png)

Rozwiń węzeł **SchoolModel. Store** , aby wyświetlić tabele, rozwiń węzeł **tabele/widoki** , aby wyświetlić tabele, a następnie rozwiń pozycję **kurs** , aby wyświetlić kolumny w tabeli.

[![Image19](the-entity-framework-and-aspnet-getting-started-part-1/_static/image54.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image53.png)

Rozwiń węzeł **SchoolModel**, rozwiń węzeł **typy jednostek**, a następnie rozwiń węzeł **kurs** , aby wyświetlić jednostki i właściwości w jednostkach.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-1/_static/image56.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image55.png)

W okienku Projektant lub **przeglądarka modeli** można zobaczyć, jak Entity Framework wiążą obiekty dwóch modeli. Kliknij prawym przyciskiem myszy jednostkę `Person` i wybierz pozycję **Mapowanie tabeli**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-1/_static/image58.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image57.png)

Spowoduje to otwarcie okna **szczegóły mapowania** . Należy zauważyć, że to okno pozwala zobaczyć, że kolumna bazy danych `FirstName` jest zamapowana na `FirstMidName`, co oznacza, że nazwa została zmieniona na wartość w modelu danych.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-1/_static/image60.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image59.png)

Entity Framework używa XML do przechowywania informacji o bazie danych, modelu danych i mapowań między nimi. Plik *SchoolModel. edmx* jest w rzeczywistości plikiem XML zawierającym te informacje. Projektant renderuje informacje w formacie graficznym, ale można również wyświetlić plik jako XML, klikając prawym przyciskiem myszy plik *. edmx* w **Eksplorator rozwiązań**, klikając polecenie **Otwórz za pomocą**i wybierając **Edytor XML (tekst)** . (Projektant modelu danych i Edytor XML są tylko dwoma różnymi sposobami otwierania tego samego pliku i pracy z nim, dzięki czemu nie można otworzyć projektanta i otworzyć go w edytorze XML w tym samym czasie).

Została utworzona witryna sieci Web, baza danych i model danych. W następnym instruktażu zaczniesz pracę z danymi przy użyciu modelu danych i kontrolki `EntityDataSource` ASP.NET.

> [!div class="step-by-step"]
> [Dalej](the-entity-framework-and-aspnet-getting-started-part-2.md)

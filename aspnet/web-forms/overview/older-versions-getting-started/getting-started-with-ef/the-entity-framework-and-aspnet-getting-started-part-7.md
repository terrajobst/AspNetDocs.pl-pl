---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
title: Wprowadzenie z Entity Framework 4,0 Database First i ASP.NET 4 Web Forms — część 7 | Microsoft Docs
author: tdykstra
description: Przykładowa aplikacja internetowa Contoso University pokazuje, jak tworzyć aplikacje ASP.NET Web Forms przy użyciu Entity Framework. Przykładowa aplikacja to...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: f8afb245-b705-419c-8790-0b295e90d5e2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
msc.type: authoredcontent
ms.openlocfilehash: 18d4b44c5e23fd6942c3adf48a33a5602e6df6d0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78603433"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-7"></a>Wprowadzenie z Entity Framework 4,0 Database First i ASP.NET 4 Web Forms — część 7

Autor [Dykstra](https://github.com/tdykstra)

> Przykładowa aplikacja internetowa Contoso University pokazuje, jak tworzyć aplikacje ASP.NET Web Forms przy użyciu Entity Framework 4,0 i programu Visual Studio 2010. Aby uzyskać informacje na temat serii samouczków, zobacz [pierwszy samouczek w serii](the-entity-framework-and-aspnet-getting-started-part-1.md)

## <a name="using-stored-procedures"></a>korzystanie z procedur składowanych

W poprzednim samouczku zaimplementowano wzorzec dziedziczenia dla hierarchii. W tym samouczku przedstawiono sposób korzystania z procedur składowanych w celu uzyskania większej kontroli nad dostępem do bazy danych.

Entity Framework pozwala określić, że należy używać procedur składowanych do uzyskiwania dostępu do bazy danych. Dla dowolnego typu jednostki można określić procedurę przechowywaną do użycia podczas tworzenia, aktualizowania lub usuwania jednostek tego typu. Następnie w modelu danych można dodać odwołania do procedur składowanych, których można użyć do wykonywania zadań, takich jak pobieranie zestawów jednostek.

Korzystanie z procedur składowanych jest typowym wymaganiem dostępu do bazy danych. W niektórych przypadkach administrator bazy danych może wymagać, aby wszyscy dostęp do bazy danych przeszedł procedury składowane ze względów bezpieczeństwa. W innych przypadkach możesz chcieć utworzyć logikę biznesową do niektórych procesów, których Entity Framework używa podczas aktualizacji bazy danych. Na przykład po usunięciu jednostki można skopiować ją do bazy danych archiwum. Lub przy każdej aktualizacji wiersza można chcieć napisać wiersz do tabeli rejestrowania, która rejestruje, kto wprowadził zmianę. Zadania te można wykonywać w procedurze składowanej, która jest wywoływana za każdym razem, gdy Entity Framework usuwa jednostkę lub aktualizuje jednostkę.

Tak jak w poprzednim samouczku, nie utworzysz żadnych nowych stron. Zamiast tego zmienisz sposób, w jaki Entity Framework uzyskuje dostęp do bazy danych dla niektórych stron, które zostały już utworzone.

W tym samouczku utworzysz procedury składowane w bazie danych na potrzeby wstawiania `Student` i `Instructor` jednostek. Dodasz je do modelu danych i określisz, że Entity Framework powinny używać ich do dodawania jednostek `Student` i `Instructor` do bazy danych. Utworzysz również procedurę składowaną, której można użyć do pobrania jednostek `Course`.

## <a name="creating-stored-procedures-in-the-database"></a>Tworzenie procedur składowanych w bazie danych

(Jeśli używasz pliku *szkoły. mdf* z projektu dostępnego do pobrania w ramach tego samouczka, możesz pominąć tę sekcję, ponieważ procedury składowane już istnieją).

W **Eksplorator serwera**rozwiń węzeł *szkoła. mdf*, kliknij prawym przyciskiem myszy pozycję **procedury składowane**i wybierz polecenie **Dodaj nową procedurę składowaną**.

[![image15](the-entity-framework-and-aspnet-getting-started-part-7/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image1.png)

Skopiuj poniższe instrukcje SQL i wklej je do okna procedury składowanej, zastępując procedurę składowaną szkieletowo.

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample1.sql)]

[![image14](the-entity-framework-and-aspnet-getting-started-part-7/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image3.png)

jednostki `Student` mają cztery właściwości: `PersonID`, `LastName`, `FirstName`i `EnrollmentDate`. Baza danych automatycznie generuje wartość identyfikatora, a procedura składowana akceptuje parametry pozostałe trzy. Procedura składowana zwraca wartość klucza rekordu nowego wiersza, dzięki czemu Entity Framework może śledzić, że w wersji jednostki przechowywanej w pamięci.

Zapisz i Zamknij okno procedury składowanej.

Utwórz procedurę składowaną `InsertInstructor` w ten sam sposób, korzystając z następujących instrukcji SQL:

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample2.sql)]

Utwórz `Update` procedury składowane dla jednostek `Student` i `Instructor`. (Baza danych ma już `DeletePerson` procedury składowanej, która będzie działała zarówno dla `Instructor`, jak i `Student` jednostek.)

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample3.sql)]

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample4.sql)]

W tym samouczku zamapujesz wszystkie trzy funkcje — INSERT, Update i DELETE--dla każdego typu jednostki. Entity Framework w wersji 4 pozwala zmapować tylko jedną lub dwie z tych funkcji na procedury składowane bez mapowania innych, z wyjątkiem sytuacji, gdy funkcja aktualizacji jest zamapowana, ale nie funkcja usuwania, Entity Framework zgłosi wyjątek podczas podjęto próbę usunięcia jednostki. W Entity Framework w wersji 3,5 nie ma znacznie dużej elastyczności podczas mapowania procedur składowanych: Jeśli zamapowano jedną funkcję, musisz zamapować wszystkie trzy.

Aby utworzyć procedurę przechowywaną, która odczytuje dane, a nie aktualizuje, należy utworzyć jeden, który wybiera wszystkie jednostki `Course` przy użyciu następujących instrukcji SQL:

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample5.sql)]

## <a name="adding-the-stored-procedures-to-the-data-model"></a>Dodawanie procedur składowanych do modelu danych

Procedury składowane są teraz zdefiniowane w bazie danych, ale należy je dodać do modelu danych, aby udostępnić je Entity Framework. Otwórz *SchoolModel. edmx*, kliknij prawym przyciskiem myszy powierzchnię projektu i wybierz polecenie **Aktualizuj model z bazy danych**. Na karcie **Dodaj** okna dialogowego **Wybierz obiekty bazy danych** rozwiń węzeł **procedury składowane**, wybierz nowo utworzone procedury składowane i `DeletePerson` procedurę składowaną, a następnie kliknij przycisk **Zakończ**.

[![image20](the-entity-framework-and-aspnet-getting-started-part-7/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image5.png)

## <a name="mapping-the-stored-procedures"></a>Mapowanie procedur składowanych

W projektancie modeli danych kliknij prawym przyciskiem myszy jednostkę `Student` i wybierz pozycję **mapowanie procedury składowanej**.

[![image21](the-entity-framework-and-aspnet-getting-started-part-7/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image7.png)

Zostanie wyświetlone okno **szczegóły mapowania** , w którym można określić procedury składowane, których Entity Framework ma używać do wstawiania, aktualizowania i usuwania jednostek tego typu.

[![image22](the-entity-framework-and-aspnet-getting-started-part-7/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image9.png)

Ustaw funkcję **INSERT** na **InsertStudent**. W oknie zostanie wyświetlona lista parametrów procedury składowanej, z których każdy musi być zamapowany na Właściwość jednostki. Dwa z nich są mapowane automatycznie, ponieważ nazwy są takie same. Brak właściwości Entity o nazwie `FirstName`, więc musisz ręcznie wybrać `FirstMidName` z listy rozwijanej, która zawiera dostępne właściwości jednostki. (Dzieje się tak, ponieważ zmieniono nazwę właściwości `FirstName` na `FirstMidName` w pierwszym samouczku).

[![image23](the-entity-framework-and-aspnet-getting-started-part-7/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image11.png)

W tym samym oknie **szczegóły mapowania** mapuj funkcję `Update` na procedurę składowaną `UpdateStudent` (Upewnij się, że określono `FirstMidName` jako wartość parametru dla `FirstName`, jak w przypadku procedury składowanej `Insert`) i funkcję `Delete` do procedury składowanej `DeletePerson`.

[![image01](the-entity-framework-and-aspnet-getting-started-part-7/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image13.png)

Postępuj zgodnie z tą samą procedurą, aby zmapować procedury składowane INSERT, Update i DELETE dla instruktorów do jednostki `Instructor`.

[![image02](the-entity-framework-and-aspnet-getting-started-part-7/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image15.png)

W przypadku procedur składowanych, które odczytują, a nie aktualizacji danych, należy użyć okna **przeglądarki modelu** do mapowania procedury składowanej na typ jednostki, która zwraca. W projektancie modeli danych kliknij prawym przyciskiem myszy powierzchnię projektu i wybierz pozycję **przeglądarka modeli**. Otwórz węzeł **SchoolModel. Store** , a następnie otwórz węzeł **procedury składowane** . Następnie kliknij prawym przyciskiem myszy `GetCourses` procedury składowanej i wybierz polecenie **Dodaj Import funkcji**.

[![image24](the-entity-framework-and-aspnet-getting-started-part-7/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image17.png)

W oknie dialogowym **Dodaj Import funkcji** w obszarze **zwraca kolekcję** wybranych **jednostek**, a następnie wybierz `Course` jako zwracany typ jednostki. Gdy skończysz, kliknij przycisk **OK**. Zapisz i zamknij plik *. edmx* .

[![image25](the-entity-framework-and-aspnet-getting-started-part-7/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image19.png)

## <a name="using-insert-update-and-delete-stored-procedures"></a>Korzystanie z procedur składowanych INSERT, Update i DELETE

Procedury składowane służące do wstawiania, aktualizowania i usuwania danych są automatycznie używane przez Entity Framework po dodaniu ich do modelu danych i zamapowaniu ich do odpowiednich jednostek. Teraz można uruchomić stronę *StudentsAdd. aspx* i za każdym razem, gdy tworzysz nowego ucznia, Entity Framework będzie używać `InsertStudent` procedury składowanej, aby dodać nowy wiersz do tabeli `Student`.

[![image03](the-entity-framework-and-aspnet-getting-started-part-7/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image21.png)

Uruchom stronę *uczniów. aspx* , a nowy student zostanie wyświetlony na liście.

[![image04](the-entity-framework-and-aspnet-getting-started-part-7/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image23.png)

Zmień nazwę, aby sprawdzić, czy działa funkcja Update, a następnie usuń uczniów, aby sprawdzić, czy funkcja delete działa.

[![image05](the-entity-framework-and-aspnet-getting-started-part-7/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image25.png)

## <a name="using-select-stored-procedures"></a>Korzystanie z procedur składowanych SELECT

Entity Framework nie uruchamia automatycznie procedur składowanych, takich jak `GetCourses`, i nie można ich używać z kontrolką `EntityDataSource`. Aby ich używać, należy wywołać je z kodu.

Otwórz plik *InstructorsCourses.aspx.cs* . Metoda `PopulateDropDownLists` używa zapytania LINQ-to-Entities do pobrania wszystkich jednostek kursu, dzięki czemu może przechodzić przez listę i określić, które z nich jest przypisane i które są nieprzypisane:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample6.cs)]

Zastąp ten kod następującym kodem:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample7.cs)]

Strona używa teraz `GetCourses` procedury składowanej w celu pobrania listy wszystkich kursów. Uruchom stronę, aby upewnić się, że działa tak jak wcześniej.

(Właściwości nawigacji jednostek pobranych przez procedurę składowaną mogą nie zostać automatycznie wypełnione danymi związanymi z tymi jednostkami, w zależności od `ObjectContext` domyślnych ustawień. Aby uzyskać więcej informacji, zobacz [ładowanie obiektów pokrewnych](https://msdn.microsoft.com/library/bb896272.aspx) w bibliotece MSDN.

W następnym samouczku dowiesz się, jak korzystać z funkcji danych dynamicznych w celu ułatwienia programowania i testowania formatowania danych oraz reguł walidacji. Zamiast określania dla każdej reguły strony sieci Web, takiej jak ciągi formatu danych i czy pole jest wymagane, można określić takie reguły w metadanych modelu danych i są one automatycznie stosowane na każdej stronie.

> [!div class="step-by-step"]
> [Poprzednie](the-entity-framework-and-aspnet-getting-started-part-6.md)
> [dalej](the-entity-framework-and-aspnet-getting-started-part-8.md)

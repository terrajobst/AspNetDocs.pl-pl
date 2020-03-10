---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
title: Wprowadzenie z Entity Framework 4,0 Database First i ASP.NET 4 Web Forms — część 6 | Microsoft Docs
author: tdykstra
description: Przykładowa aplikacja internetowa Contoso University pokazuje, jak tworzyć aplikacje ASP.NET Web Forms przy użyciu Entity Framework. Przykładowa aplikacja to...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: 994a5496-c648-4830-b03c-55bb43f325d2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
msc.type: authoredcontent
ms.openlocfilehash: 8bfbe74f90eb03ea9aab7610842ef2578e80d113
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78564226"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-6"></a>Wprowadzenie z Entity Framework 4,0 Database First i ASP.NET 4 Web Forms — część 6

Autor [Dykstra](https://github.com/tdykstra)

> Przykładowa aplikacja internetowa Contoso University pokazuje, jak tworzyć aplikacje ASP.NET Web Forms przy użyciu Entity Framework 4,0 i programu Visual Studio 2010. Aby uzyskać informacje na temat serii samouczków, zobacz [pierwszy samouczek w serii](the-entity-framework-and-aspnet-getting-started-part-1.md)

## <a name="implementing-table-per-hierarchy-inheritance"></a>Implementowanie dziedziczenia na poziomie tabeli

W poprzednim samouczku pracowałeś nad powiązanymi danymi przez dodawanie i usuwanie relacji oraz Dodawanie nowej jednostki, która miała relację z istniejącą jednostką. W tym samouczku pokazano, jak zaimplementować dziedziczenie w modelu danych.

W programowaniu zorientowanym obiektowo można użyć dziedziczenia, aby ułatwić pracę z powiązanymi klasami. Można na przykład utworzyć klasy `Instructor` i `Student`, które pochodzą z klasy bazowej `Person`. Można utworzyć te same rodzaje struktur dziedziczenia między jednostkami w Entity Framework.

W tej części samouczka nie utworzysz żadnych nowych stron sieci Web. Zamiast tego należy dodać jednostki pochodne do modelu danych i zmodyfikować istniejące strony, aby użyć nowych jednostek.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Dziedziczenie na poziomie hierarchii w zależności od typu tabeli

Baza danych może przechowywać informacje o obiektach pokrewnych w jednej tabeli lub w wielu tabelach. Na przykład w bazie danych `School` tabela `Person` zawiera informacje o studentach i instruktorach w pojedynczej tabeli. Niektóre kolumny mają zastosowanie tylko do instruktorów (`HireDate`), niektórych tylko dla uczniów (`EnrollmentDate`) i niektórych do obu (`LastName`, `FirstName`).

[![image11](the-entity-framework-and-aspnet-getting-started-part-6/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image1.png)

Entity Framework można skonfigurować do tworzenia jednostek `Instructor` i `Student` dziedziczących z jednostki `Person`. Ten wzorzec generowania struktury dziedziczenia jednostek z pojedynczej tabeli bazy danych jest nazywany dziedziczeniem *na hierarchię* (TPH).

W przypadku kursów baza danych `School` używa innego wzorca. Kursy online i kursy w miejscu są przechowywane w oddzielnych tabelach, z których każdy ma klucz obcy wskazujący tabelę `Course`. Informacje wspólne dla obu typów kursów są przechowywane tylko w tabeli `Course`.

[![image12](the-entity-framework-and-aspnet-getting-started-part-6/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image3.png)

Model danych Entity Framework można skonfigurować tak, aby jednostki `OnlineCourse` i `OnsiteCourse` dziedziczyły z jednostki `Course`. Ten wzorzec generowania struktury dziedziczenia jednostek z oddzielnych tabel dla każdego typu, z każdą oddzielną tabelą odwołującą się do tabeli, która przechowuje dane wspólne dla wszystkich typów, jest nazywana dziedziczeniem *tabeli na typ* (TPT).

Wzorce dziedziczenia TPH ogólnie zapewniają lepszą wydajność w Entity Framework niż wzorce dziedziczenia TPT, ponieważ wzorce TPT mogą powodować złożone zapytania sprzężenia. W tym instruktażu przedstawiono sposób implementacji dziedziczenia TPH. Można to zrobić, wykonując następujące czynności:

- Utwórz `Instructor` i `Student` typy jednostek, które pochodzą z `Person`.
- Przenieś właściwości, które odnoszą się do jednostek pochodnych z jednostki `Person` do jednostek pochodnych.
- Ustaw ograniczenia dotyczące właściwości w typach pochodnych.
- Uczyń jednostkę `Person` jednostką abstrakcyjną.
- Mapuj każdą jednostkę pochodną do tabeli `Person` z warunkiem określającym, czy wiersz `Person` reprezentuje typ pochodny.

## <a name="adding-instructor-and-student-entities"></a>Dodawanie instruktorów i jednostek uczniów

Otwórz plik <em>SchoolModel. edmx</em> , kliknij prawym przyciskiem myszy niezajęty obszar w projektancie, wybierz pozycję <strong>Dodaj</strong>, a następnie wybierz pozycję <strong>Jednostka</strong><em>.</em>

[![image01](the-entity-framework-and-aspnet-getting-started-part-6/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image5.png)

W oknie dialogowym **Dodaj jednostkę** nazwij jednostkę `Instructor` i ustaw jej opcję **typ podstawowy** na `Person`.

[![image02](the-entity-framework-and-aspnet-getting-started-part-6/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image7.png)

Kliknij przycisk **OK**. Projektant tworzy jednostkę `Instructor`, która pochodzi od jednostki `Person`. Nowa jednostka nie ma jeszcze żadnych właściwości.

[![image03](the-entity-framework-and-aspnet-getting-started-part-6/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image9.png)

Powtórz procedurę, aby utworzyć jednostkę `Student`, która również pochodzi z `Person`.

Tylko Instruktorzy mają daty zatrudnienia, dlatego należy przenieść tę właściwość z jednostki `Person` do jednostki `Instructor`. W jednostce `Person` kliknij prawym przyciskiem myszy Właściwość `HireDate` i kliknij polecenie **Wytnij**. Następnie kliknij prawym przyciskiem myszy **Właściwości** w jednostce `Instructor` i kliknij przycisk **Wklej**.

[![image04](the-entity-framework-and-aspnet-getting-started-part-6/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image11.png)

Data zatrudnienia jednostki `Instructor` nie może mieć wartości null. Kliknij prawym przyciskiem myszy Właściwość `HireDate`, kliknij polecenie **Właściwości**, a następnie w oknie **Właściwości** Zmień `Nullable` do `False`.

[![image05](the-entity-framework-and-aspnet-getting-started-part-6/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image13.png)

Powtórz procedurę, aby przenieść Właściwość `EnrollmentDate` z jednostki `Person` do jednostki `Student`. Upewnij się, że ustawiono również `Nullable`, aby `False` Właściwość `EnrollmentDate`.

Teraz, gdy jednostka `Person` ma tylko właściwości wspólne dla `Instructor` i `Student` jednostek (poza właściwościami nawigacji, które nie są przesuwane), jednostka może być używana tylko jako jednostka podstawowa w strukturze dziedziczenia. W związku z tym należy upewnić się, że nigdy nie jest traktowany jako jednostka niezależna. Kliknij prawym przyciskiem myszy jednostkę `Person`, wybierz pozycję **Właściwości**, a następnie w oknie **Właściwości** Zmień wartość właściwości **abstract** na **true**.

[![image06](the-entity-framework-and-aspnet-getting-started-part-6/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image15.png)

## <a name="mapping-instructor-and-student-entities-to-the-person-table"></a>Mapowanie instruktorów i jednostek uczniów do tabeli Person

Teraz musisz popowiedzieć Entity Framework, jak rozróżnić `Instructor` i `Student` jednostek w bazie danych.

Kliknij prawym przyciskiem myszy jednostkę `Instructor` i wybierz pozycję **Mapowanie tabeli**. W oknie **szczegóły mapowania** kliknij pozycję **Dodaj tabelę lub widok** i wybierz **osobę**.

[![image07](the-entity-framework-and-aspnet-getting-started-part-6/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image17.png)

Kliknij pozycję **Dodaj warunek**, a następnie wybierz pozycję **HireDate**.

[![image09](the-entity-framework-and-aspnet-getting-started-part-6/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image19.png)

Zmień **operator** na **is** i **Value/Property na wartość** **not null**.

[![image10](the-entity-framework-and-aspnet-getting-started-part-6/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image21.png)

Powtórz procedurę dla jednostki `Students`, określając, że ta jednostka mapuje do tabeli `Person`, gdy kolumna `EnrollmentDate` nie ma wartości null. Następnie Zapisz i Zamknij model danych.

Skompiluj projekt, aby utworzyć nowe jednostki jako klasy i udostępnić je w projektancie.

## <a name="using-the-instructor-and-student-entities"></a>Korzystanie z instruktorów i jednostek uczniów

Po utworzeniu stron sieci Web, które pracują z danymi ucznia i instruktora, dane są powiązane z zestawem jednostek `Person` i filtrowane według właściwości `HireDate` lub `EnrollmentDate` w celu ograniczenia danych zwracanych do uczniów lub instruktorów. Jednak teraz po powiązaniu każdej kontroli źródła danych z zestawem jednostek `Person` można określić, że należy wybrać tylko `Student` lub `Instructor` typy jednostek. Ponieważ Entity Framework wie, jak odróżnić uczniów i instruktorów w zestawie jednostek `Person`, można usunąć ustawienia właściwości `Where` wprowadzone ręcznie.

W projektancie programu Visual Studio można określić typ jednostki, który powinien zostać wybrany przez formant `EntityDataSource` w polu listy rozwijanej **EntityTypeFilter** kreatora `Configure Data Source`, jak pokazano w poniższym przykładzie.

[![image13](the-entity-framework-and-aspnet-getting-started-part-6/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image23.png)

A w oknie **Właściwości** można usunąć wartości klauzule `Where`, które nie są już potrzebne, jak pokazano w poniższym przykładzie.

[![image14](the-entity-framework-and-aspnet-getting-started-part-6/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image25.png)

Jednak ze względu na to, że znaczniki `EntityDataSource` formantów zostały zmienione, aby użyć atrybutu `ContextTypeName`, nie można uruchomić kreatora **konfiguracji źródła danych** na `EntityDataSource` kontrolkach, które zostały już utworzone. W związku z tym wprowadzisz wymagane zmiany przez zmianę znaczników.

Otwórz stronę *uczniów. aspx* . W kontrolce `StudentsEntityDataSource` Usuń atrybut `Where` i Dodaj atrybut `EntityTypeFilter="Student"`. Znacznik będzie teraz podobny do poniższego przykładu:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample1.aspx)]

Ustawienie atrybutu `EntityTypeFilter` gwarantuje, że kontrolka `EntityDataSource` będzie wybierać tylko określony typ jednostki. Jeśli chcesz pobrać typy jednostek `Student` i `Instructor`, nie ustawisz tego atrybutu. (Istnieje możliwość pobrania wielu typów jednostek z jedną kontrolką `EntityDataSource` tylko wtedy, gdy jest używana kontrolka dostępu do danych tylko do odczytu. Jeśli używasz kontrolki `EntityDataSource` do wstawiania, aktualizowania lub usuwania jednostek, a jeśli obiekt, z którym jest powiązany, może zawierać wiele typów, możesz korzystać tylko z jednego typu jednostki i musisz ustawić ten atrybut.

Powtórz procedurę dla kontrolki `SearchEntityDataSource`, z wyjątkiem usunięcia tylko części atrybutu `Where`, który wybiera `Student` jednostki zamiast całkowicie usunąć właściwość. Tag otwierający kontrolki będzie teraz wyglądać podobnie do poniższego przykładu:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample2.aspx)]

Uruchom stronę, aby upewnić się, że nadal działa tak jak wcześniej.

[![image15](the-entity-framework-and-aspnet-getting-started-part-6/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image27.png)

Zaktualizuj następujące strony utworzone we wcześniejszych samouczkach, tak aby korzystały z nowych `Student` i `Instructor` jednostek zamiast `Person` jednostek, a następnie uruchom je, aby sprawdzić, czy działają tak jak wcześniej:

- W *StudentsAdd. aspx*Dodaj `EntityTypeFilter="Student"` do kontrolki `StudentsEntityDataSource`. Znacznik będzie teraz podobny do poniższego przykładu: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample3.aspx)]

    [![image16](the-entity-framework-and-aspnet-getting-started-part-6/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image29.png)
- W oknie *about. aspx*Dodaj `EntityTypeFilter="Student"` do kontrolki `StudentStatisticsEntityDataSource` i Usuń `Where="it.EnrollmentDate is not null"`. Znacznik będzie teraz podobny do poniższego przykładu: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample4.aspx)]

    [![image17](the-entity-framework-and-aspnet-getting-started-part-6/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image31.png)
- W oknie *Instruktorzy. aspx* i *InstructorsCourses. aspx*Dodaj `EntityTypeFilter="Instructor"` do kontrolki `InstructorsEntityDataSource` i Usuń `Where="it.HireDate is not null"`. Znaczniki w *instruktorach. aspx* są teraz podobne do następujących: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample5.aspx)]

    [![image18](the-entity-framework-and-aspnet-getting-started-part-6/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image33.png)

    Adiustacja w *InstructorsCourses. aspx* będzie teraz podobna do poniższego przykładu:

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample6.aspx)]

    [![image19](the-entity-framework-and-aspnet-getting-started-part-6/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image35.png)

W wyniku tych zmian Ulepszono łatwość utrzymania aplikacji firmy Contoso na kilka sposobów. Przeniesiono wybór i logikę walidacji z warstwy interfejsu użytkownika (znaczników *. aspx* ) i uczynić ją integralną częścią warstwy dostępu do danych. Pozwala to na odizolowanie kodu aplikacji od zmian, które mogą być wprowadzane w przyszłości do schematu bazy danych lub modelu danych. Na przykład możesz zdecydować, że uczniowie mogą być zatrudniane jako pomoce dla nauczycieli i w związku z tym otrzymają datę zatrudnienia. Następnie można dodać nową właściwość w celu odróżnienia uczniów od instruktorów i zaktualizowania modelu danych. Nie trzeba zmieniać kodu w aplikacji sieci Web, chyba że chcesz wyświetlić datę zatrudnienia dla uczniów. Kolejną zaletą dodawania jednostek `Instructor` i `Student` jest to, że kod jest bardziej łatwo zrozumiały niż w przypadku `Person` obiektów, które były rzeczywiście uczniami lub instruktorami.

Już widzisz jeden ze sposobów implementacji wzorca dziedziczenia w Entity Framework. W poniższym samouczku dowiesz się, jak używać procedur składowanych w celu zapewnienia większej kontroli nad sposobem uzyskiwania dostępu do bazy danych przez Entity Framework.

> [!div class="step-by-step"]
> [Poprzednie](the-entity-framework-and-aspnet-getting-started-part-5.md)
> [dalej](the-entity-framework-and-aspnet-getting-started-part-7.md)

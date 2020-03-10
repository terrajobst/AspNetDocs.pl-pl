---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
title: Wprowadzenie z Entity Framework 4,0 Database First i ASP.NET 4 Web Forms — część 3 | Microsoft Docs
author: tdykstra
description: Przykładowa aplikacja internetowa Contoso University pokazuje, jak tworzyć aplikacje ASP.NET Web Forms przy użyciu Entity Framework. Przykładowa aplikacja to...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: ccdc3f8c-2568-40a7-8f8b-3c23d2e05388
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
msc.type: authoredcontent
ms.openlocfilehash: 88debb11a9157dce9ff000b1cb459b876dbceaf3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78643249"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-3"></a>Wprowadzenie z Entity Framework 4,0 Database First i ASP.NET 4 Web Forms — część 3

Autor [Dykstra](https://github.com/tdykstra)

> Przykładowa aplikacja internetowa Contoso University pokazuje, jak tworzyć aplikacje ASP.NET Web Forms przy użyciu Entity Framework 4,0 i programu Visual Studio 2010. Aby uzyskać informacje na temat serii samouczków, zobacz [pierwszy samouczek w serii](the-entity-framework-and-aspnet-getting-started-part-1.md)

## <a name="filtering-ordering-and-grouping-data"></a>Filtrowanie, porządkowanie i grupowanie danych

W poprzednim samouczku użyto kontrolki `EntityDataSource`, aby wyświetlić i edytować dane. W tym samouczku będziesz filtrować, zamawiać i grupować dane. Gdy to zrobisz, ustawiając właściwości kontrolki `EntityDataSource`, Składnia różni się od innych kontrolek źródła danych. Jak widać, można jednak zminimalizować te różnice przy użyciu kontrolki `QueryExtender`.

Zmienisz stronę *uczniowie. aspx* , aby odfiltrować uczniów, posortować według nazwy i wyszukać nazwę. Zmienisz również stronę *kursy. aspx* , aby wyświetlić kursy dla wybranego działu i wyszukać kursy według nazwy. Na koniec dodasz statystyki uczniów do strony *about. aspx* .

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image1.png)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image3.png)

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image5.png)

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image7.png)

## <a name="using-the-entitydatasource-where-property-to-filter-data"></a>Używanie właściwości "Where" obiektu EntityDataSource do filtrowania danych

Otwórz stronę *uczniów. aspx* utworzoną w poprzednim samouczku. Zgodnie z aktualnie skonfigurowanym formantem `GridView` na stronie są wyświetlane wszystkie nazwy z zestawu jednostek `People`. Jednak chcesz wyświetlić tylko uczniów, których można znaleźć, wybierając `Person` jednostki, które mają daty rejestracji o wartości innej niż null.

Przejdź do widoku **projektu** i wybierz formant `EntityDataSource`. W oknie **Właściwości** ustaw właściwość `Where` na `it.EnrollmentDate is not null`.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-3/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image9.png)

Składnia używana we właściwości `Where` formantu `EntityDataSource` jest Entity SQL. Entity SQL jest podobny do języka Transact-SQL, ale jest dostosowany do użycia z jednostkami, a nie obiektami bazy danych. W wyrażeniu `it.EnrollmentDate is not null`słowo `it` reprezentuje odwołanie do jednostki zwróconej przez zapytanie. W związku z tym `it.EnrollmentDate` odnosi się do właściwości `EnrollmentDate` jednostki `Person`, którą zwróci formant `EntityDataSource`.

Uruchom stronę. Lista uczniów zawiera teraz tylko uczniów. (Nie są wyświetlane żadne wiersze, w których nie ma daty rejestracji).

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image11.png)

## <a name="using-the-entitydatasource-orderby-property-to-order-data"></a>Używanie właściwości "OrderBy" obiektu EntityDataSource do porządkowania danych

Ta lista powinna być również w kolejności nazw, gdy jest wyświetlana po raz pierwszy. Gdy strona *uczniów. aspx* jest nadal otwarta w widoku **projektu** , a kontrolka `EntityDataSource` nadal zaznaczona, w oknie **właściwości** ustaw właściwość **OrderBy** na `it.LastName`.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-3/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image13.png)

Uruchom stronę. Lista uczniów jest teraz uporządkowana według imienia i nazwiska.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-3/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image15.png)

## <a name="using-a-control-parameter-to-set-the-where-property"></a>Ustawianie właściwości "Where" przy użyciu parametru sterującego

Podobnie jak w przypadku innych kontrolek źródła danych, można przekazać wartości parametrów do właściwości `Where`. Na stronie *kursy. aspx* , która została utworzona w części 2 samouczka, można użyć tej metody do wyświetlenia kursów skojarzonych z działem wybieranym przez użytkownika z listy rozwijanej.

Otwórz okno *kursy. aspx* i przejdź do widoku **projektu** . Dodaj drugą kontrolkę `EntityDataSource` do strony i nadaj jej nazwę `CoursesEntityDataSource`. Połącz ją z modelem `SchoolEntities` i wybierz `Courses` jako wartość **entitySetName** .

W oknie **Właściwości** kliknij wielokropek w polu właściwości **WHERE** . (Upewnij się, że kontrolka `CoursesEntityDataSource` jest nadal zaznaczona przed użyciem okna **Właściwości** .)

[![Image06](the-entity-framework-and-aspnet-getting-started-part-3/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image17.png)

Zostanie wyświetlone okno dialogowe **Edytor wyrażeń** . W tym oknie dialogowym wybierz opcję **automatycznie Generuj wyrażenie WHERE na podstawie podanych parametrów**, a następnie kliknij przycisk **Dodaj parametr**. Nazwij parametr `DepartmentID`, wybierz pozycję **Kontrola** jako wartość **źródłową parametru** , a następnie wybierz pozycję **DepartmentsDropDownList** jako wartość **ControlID** .

[![Image07](the-entity-framework-and-aspnet-getting-started-part-3/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image19.png)

Kliknij pozycję **Pokaż zaawansowane właściwości**, a następnie w oknie **Właściwości** okna dialogowego **Edytor wyrażeń** Zmień właściwość `Type` na `Int32`.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-3/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image21.png)

Gdy skończysz, kliknij przycisk **OK**.

Poniżej listy rozwijanej Dodaj kontrolkę `GridView` do strony i nadaj jej nazwę `CoursesGridView`. Podłącz go do kontroli źródła danych `CoursesEntityDataSource`, kliknij przycisk **Odśwież schemat**, kliknij polecenie **Edytuj kolumny**i Usuń kolumnę `DepartmentID`. `GridView` znacznik kontroli jest podobny do następującego przykładu.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample1.aspx)]

Gdy użytkownik zmieni wybranego działu na liście rozwijanej, chcesz automatycznie zmienić listę skojarzonych kursów. Aby to zrobić, wybierz listę rozwijaną, a następnie w oknie **Właściwości** ustaw właściwość `AutoPostBack` na `True`.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-3/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image23.png)

Po zakończeniu korzystania z projektanta przejdź do widoku **źródła** i Zastąp właściwości `ConnectionString` i `DefaultContainer` nazwy kontrolki `CoursesEntityDataSource` z atrybutem `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"`. Gdy skończysz, znaczniki dla kontrolki będą wyglądać podobnie jak w poniższym przykładzie.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample2.aspx)]

Uruchom stronę i Użyj listy rozwijanej, aby wybrać różne działy. W kontrolce `GridView` są wyświetlane tylko kursy oferowane przez wybrany dział.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-3/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image25.png)

## <a name="using-the-entitydatasource-groupby-property-to-group-data"></a>Używanie właściwości EntityDataSource "GroupBy" do grupowania danych

Załóżmy, że firma Contoso University chce umieścić na stronie informacje o statystyce dla uczniów. W odróżnieniu od tej pory chcemy pokazać podział liczby uczniów według daty, którą zarejestrowałeś.

Otwórz *Informacje o. aspx*i w widoku **źródła** Zamień istniejącą zawartość kontrolki `BodyContent` na "statystyki treści ucznia" między tagami `h2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample3.aspx)]

Po nagłówku Dodaj kontrolkę `EntityDataSource` i nadaj jej nazwę `StudentStatisticsEntityDataSource`. Połącz ją z `SchoolEntities`, wybierz zestaw jednostek `People` i pozostaw pole **wyboru** w Kreatorze bez zmian. W oknie **Właściwości** ustaw następujące właściwości:

- Aby filtrować tylko uczniów, ustaw właściwość `Where` na `it.EnrollmentDate is not null`.
- Aby pogrupować wyniki według daty rejestracji, ustaw właściwość `GroupBy` na `it.EnrollmentDate`.
- Aby wybrać datę rejestracji i liczbę uczniów, ustaw właściwość `Select` na `it.EnrollmentDate, Count(it.EnrollmentDate) AS NumberOfStudents`.
- Aby określić kolejność wyników według daty rejestracji, ustaw właściwość `OrderBy` na `it.EnrollmentDate`.

W widoku **Źródło** Zastąp właściwości `ConnectionString` i `DefaultContainer` nazwa właściwością `ContextTypeName`. `EntityDataSource` znaczników kontrolki jest teraz podobna do poniższego przykładu.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample4.aspx)]

Składnia właściwości `Select`, `GroupBy`i `Where` przypomina język Transact-SQL z wyjątkiem słowa kluczowego `it`, które określa bieżącą jednostkę.

Dodaj następujący znacznik, aby utworzyć formant `GridView`, aby wyświetlić dane.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample5.aspx)]

Uruchom stronę, aby wyświetlić listę pokazującą liczbę studentów według daty rejestracji.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image27.png)

## <a name="using-the-queryextender-control-for-filtering-and-ordering"></a>Filtrowanie i porządkowanie przy użyciu formantu QueryExtender

Formant `QueryExtender` zapewnia sposób określania filtrowania i sortowania w znacznikach. Składnia jest niezależna od używanego systemu zarządzania bazami danych (DBMS). Jest ona również ogólnie niezależna od Entity Framework, z wyjątkiem tego, że składnia używana dla właściwości nawigacji jest unikatowa dla Entity Framework.

W tej części samouczka użyjesz kontrolki `QueryExtender` do filtrowania i porządkowania danych, a jednym z pól order-by będzie właściwość nawigacji.

(Jeśli wolisz używać kodu zamiast znacznika do rozszerania zapytań, które są automatycznie generowane przez formant `EntityDataSource`, możesz to zrobić przez obsługę zdarzenia `QueryCreated`. Jest to sposób, w jaki formant `QueryExtender` rozszerza również zapytania sterujące `EntityDataSource`.)

Otwórz stronę *kursy. aspx* i poniżej znacznika, który dodałeś wcześniej, Wstaw następujące znaczniki, aby utworzyć nagłówek, pole tekstowe służące do wprowadzania ciągów wyszukiwania, przycisk wyszukiwania i formant `EntityDataSource`, który jest powiązany z zestawem jednostek `Courses`.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample6.aspx)]

Zauważ, że właściwość `Include` kontrolki `EntityDataSource` jest ustawiona na `Department`. W bazie danych tabela `Course` nie zawiera nazwy działu; zawiera `DepartmentID` kolumnę klucza obcego. W przypadku bezpośredniego wykonywania zapytania względem bazy danych w celu uzyskania nazwy działu wraz z danymi o kursie należy dołączyć tabele `Course` i `Department`. Ustawiając właściwość `Include` na `Department`, należy określić, że Entity Framework powinien wykonać zadania związane z uzyskiwaniem powiązanej jednostki `Department`, gdy pobiera `Course` jednostkę. Jednostka `Department` jest następnie przechowywana we właściwości nawigacji `Department` jednostki `Course`. (Domyślnie Klasa `SchoolEntities`, która została wygenerowana przez projektanta modelu danych, pobiera powiązane dane, gdy jest to potrzebne, i powiązano kontrolę źródła danych z tą klasą, więc ustawienie właściwości `Include` nie jest konieczne. Jednak ustawienie to zwiększa wydajność strony, ponieważ w przeciwnym razie Entity Framework może nawiązywać oddzielne wywołania do bazy danych w celu pobierania danych dla jednostek `Course` i powiązanych jednostek `Department`.)

Po utworzeniu kontrolki `EntityDataSource` Wstaw następujące znaczniki, aby utworzyć kontrolkę `QueryExtender` powiązaną z tą `EntityDataSource` kontrolką.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample7.aspx)]

Element `SearchExpression` określa, czy chcesz wybrać kursy, których tytuły pasują do wartości wprowadzonej w polu tekstowym. Porównywane są tylko znaki wprowadzone w polu tekstowym, ponieważ właściwość `SearchType` określa `StartsWith`.

`OrderByExpression` element określa, że zestaw wyników będzie uporządkowany według tytułu kursu w ramach nazwy działu. Zauważ, jak określono nazwę działu: `Department.Name`. Ponieważ skojarzenie między jednostką `Course` i jednostką `Department` to jeden-do-jednego, właściwość nawigacji `Department` zawiera jednostkę `Department`. (Jeśli była to relacja jeden do wielu, właściwość będzie zawierać kolekcję). Aby uzyskać nazwę działu, należy określić właściwość `Name` jednostki `Department`.

Na koniec Dodaj kontrolkę `GridView`, aby wyświetlić listę kursów:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample8.aspx)]

Pierwsza kolumna to pole szablonu, które wyświetla nazwę działu. Wyrażenie DataBinding określa `Department.Name`, tak jak pokazano w kontrolce `QueryExtender`.

Uruchom stronę. Początkowy ekran przedstawia listę wszystkich kursów w kolejności według działu, a następnie według tytułu kursu.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image29.png)

Wprowadź "m" i kliknij przycisk **Wyszukaj** , aby zobaczyć wszystkie kursy, których tytuły zaczynają się od "m" (wyszukiwanie nie uwzględnia wielkości liter).

[![Image12](the-entity-framework-and-aspnet-getting-started-part-3/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image31.png)

## <a name="using-the-like-operator-to-filter-data"></a>Używanie operatora "Like" do filtrowania danych

Można osiągnąć efekt podobny do `StartsWith`, `Contains`i `EndsWith` wyszukiwania `QueryExtender` kontrolki, przy użyciu operatora `Like` we właściwości `EntityDataSource` formantu `Where`. W tej części samouczka zobaczysz, jak użyć operatora `Like`, aby wyszukać uczniów według nazwy.

Otwórz *uczniów. aspx* w widoku **źródła** . Po kontrolce `GridView` Dodaj następujące znaczniki:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample9.aspx)]

Ten znacznik jest podobny do przedstawionego wcześniej, z wyjątkiem wartości właściwości `Where`. Druga część wyrażenia `Where` definiuje wyszukiwanie podciągów (`LIKE %FirstMidName% or LIKE %LastName%`), które przeszukują zarówno imię, jak i nazwisko dla dowolnego elementu w polu tekstowym.

Uruchom stronę. Początkowo zobaczysz wszystkich uczniów, ponieważ domyślna wartość parametru `StudentName` to "%".

[![Image13](the-entity-framework-and-aspnet-getting-started-part-3/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image33.png)

Wprowadź literę "g" w polu tekstowym, a następnie kliknij przycisk **Wyszukaj**. Zobaczysz listę studentów, którzy mają "g" w imieniu lub nazwisku.

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image35.png)

W poszczególnych tabelach zostały teraz wyświetlone, zaktualizowane, przefiltrowane, uporządkowane i pogrupowane dane. W następnym samouczku zaczniesz pracę z powiązanymi danymi (scenariusze główne-szczegóły).

> [!div class="step-by-step"]
> [Poprzednie](the-entity-framework-and-aspnet-getting-started-part-2.md)
> [dalej](the-entity-framework-and-aspnet-getting-started-part-4.md)

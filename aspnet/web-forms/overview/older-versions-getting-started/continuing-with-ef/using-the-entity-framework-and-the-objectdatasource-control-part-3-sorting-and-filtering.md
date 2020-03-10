---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: 'Korzystając z Entity Framework 4,0 i formantu ObjectDataSource, część 3: sortowanie i filtrowanie | Microsoft Docs'
author: tdykstra
description: Ta seria samouczków jest oparta na aplikacji sieci Web firmy Contoso University, utworzonej przez Wprowadzenie z serią samouczków Entity Framework 4,0. I...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: 603120864528b9a5ff81214270eb9a7f1b68b347
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78631671"
---
# <a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a>Używanie Entity Framework 4,0 i formantu ObjectDataSource, część 3: sortowanie i filtrowanie

Autor [Dykstra](https://github.com/tdykstra)

> Ta seria samouczków jest oparta na aplikacji sieci Web firmy Contoso University, utworzonej przez Wprowadzenie z serią samouczków [Entity Framework 4,0](https://asp.net/entity-framework/tutorials#Getting%20Started) . Jeśli wcześniej samouczki nie zostały wykonane, jako punkt wyjścia dla tego samouczka możesz [pobrać utworzoną aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) . Możesz również [pobrać aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) utworzoną przez kompletną serię samouczków. Jeśli masz pytania dotyczące samouczków, możesz je ogłosić na [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).

W poprzednim samouczku zaimplementowano wzorzec repozytorium w wielowarstwowej aplikacji sieci Web, która używa Entity Framework i kontrolki `ObjectDataSource`. W tym samouczku pokazano, jak wykonywać sortowanie i filtrowanie oraz obsłużyć scenariusze wzorca-szczegóły. Na stronie *działS. aspx* zostaną dodane następujące ulepszenia:

- Pole tekstowe umożliwiające użytkownikom wybieranie działów według nazwy.
- Lista kursów dla każdego działu, który jest wyświetlany w siatce.
- Możliwość sortowania przez kliknięcie nagłówków kolumn.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)

## <a name="adding-the-ability-to-sort-gridview-columns"></a>Dodawanie możliwości sortowania kolumn GridView

Otwórz stronę *działS. aspx* i dodaj atrybut `SortParameterName="sortExpression"` do kontrolki `ObjectDataSource` o nazwie `DepartmentsObjectDataSource`. (W dalszej części utworzysz metodę `GetDepartments`, która przyjmuje parametr o nazwie `sortExpression`.) Znaczniki dla otwierającego tagu kontrolki są teraz podobne do poniższego przykładu.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

Dodaj atrybut `AllowSorting="true"` do tagu otwierającego kontrolki `GridView`. Znaczniki dla otwierającego tagu kontrolki są teraz podobne do poniższego przykładu.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

W *Departments.aspx.cs*Ustaw domyślną kolejność sortowania, wywołując metodę `Sort` formantu `GridView` z metody `Page_Load`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

Można dodać kod, który sortuje lub filtruje w klasie logiki biznesowej lub klasy repozytorium. Jeśli zostanie to zrobione w klasie logiki biznesowej, zadania sortowania lub filtrowania będą wykonywane po pobraniu danych z bazy danych, ponieważ Klasa logiki biznesowej pracuje z obiektem `IEnumerable` zwróconym przez repozytorium. W przypadku dodania sortowania i filtrowania kodu w klasie repozytorium, gdy zostanie to wykonane przed przekonwertowaniem wyrażenia LINQ lub zapytania obiektu na obiekt `IEnumerable`, polecenia zostaną przesłane do bazy danych w celu przetworzenia, co jest zwykle bardziej wydajne. W tym samouczku zaimplementowano sortowanie i filtrowanie w taki sposób, który powoduje, że przetwarzanie zostanie wykonane przez bazę danych — czyli w repozytorium.

Aby dodać funkcję sortowania, należy dodać nową metodę do interfejsów repozytorium i klas repozytorium oraz do klasy logiki biznesowej. W pliku *ISchoolRepository.cs* Dodaj nową metodę `GetDepartments`, która przyjmuje `sortExpression` parametr, który będzie używany do sortowania listy zwracanych wydziałów:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

Parametr `sortExpression` określa kolumnę, według której ma zostać wykonane sortowanie, oraz kierunek sortowania.

Dodaj kod dla nowej metody do pliku *SchoolRepository.cs* :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

Zmień istniejącą metodę `GetDepartments` bez parametrów, aby wywołać nową metodę:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

W projekcie testowym Dodaj następującą nową metodę do *MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

Jeśli chcesz utworzyć wszystkie testy jednostkowe, które od tej metody zwracają posortowaną listę, należy posortować listę przed jej zwróceniem. Nie będziesz tworzyć testów takich jak w tym samouczku, dlatego metoda może po prostu zwrócić niesortowaną listę działów.

W pliku *SchoolBL.cs* Dodaj następującą nową metodę do klasy logiki biznesowej:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

Ten kod przekazuje parametr sortowania do metody repozytorium.

Uruchom stronę *działS. aspx* .

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)

Teraz możesz kliknąć dowolny nagłówek kolumny, aby posortować według tej kolumny. Jeśli kolumna jest już posortowana, kliknięcie nagłówka powoduje odwrócenie kierunku sortowania.

## <a name="adding-a-search-box"></a>Dodawanie pola wyszukiwania

W tej sekcji dodasz pole tekstowe wyszukiwania, połączysz je z kontrolką `ObjectDataSource` przy użyciu parametru kontrolki i dodamy metodę do klasy logiki biznesowej w celu obsługi filtrowania.

Otwórz stronę *działS. aspx* i Dodaj następujące znaczniki między nagłówkiem i pierwszą kontrolką `ObjectDataSource`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

W kontrolce `ObjectDataSource` o nazwie `DepartmentsObjectDataSource`wykonaj następujące czynności:

- Dodaj element `SelectParameters` dla parametru o nazwie `nameSearchString`, który pobiera wartość wprowadzoną w kontrolce `SearchTextBox`.
- Zmień wartość atrybutu `SelectMethod` na `GetDepartmentsByName`. (Ta metoda zostanie utworzona później).

Znaczniki dla kontrolki `ObjectDataSource` są teraz podobne do następujących:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

W *ISchoolRepository.cs*dodaj metodę `GetDepartmentsByName`, która przyjmuje zarówno parametry `sortExpression`, jak i `nameSearchString`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

W *SchoolRepository.cs*Dodaj następującą nową metodę:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

Ten kod używa metody `Where`, aby wybrać elementy, które zawierają ciąg wyszukiwania. Jeśli ciąg wyszukiwania jest pusty, zostaną zaznaczone wszystkie rekordy. Należy pamiętać, że po określeniu wywołań metod w jednej instrukcji, takiej jak (`Include`, a następnie `OrderBy`, a następnie `Where`), Metoda `Where` musi być zawsze Ostatnia.

Zmień istniejącą metodę `GetDepartments`, która przyjmuje parametr `sortExpression`, aby wywołać nową metodę:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

W *MockSchoolRepository.cs* w projekcie testowym Dodaj następującą nową metodę:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

W *SchoolBL.cs*Dodaj następującą nową metodę:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

Uruchom stronę *działu. aspx* , a następnie wprowadź ciąg wyszukiwania, aby upewnić się, że logika wyboru działa. Pozostaw pole tekstowe puste i spróbuj wykonać wyszukiwanie, aby upewnić się, że wszystkie rekordy są zwracane.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)

## <a name="adding-a-details-column-for-each-grid-row"></a>Dodawanie kolumny szczegółów dla każdego wiersza siatki

Następnie chcesz zobaczyć wszystkie kursy dla każdego działu wyświetlanego w prawej komórce siatki. W tym celu należy użyć zagnieżdżonej kontrolki `GridView` i powiązać ją z danymi z `Courses` właściwości nawigacji jednostki `Department`.

Otwórz *działy. aspx* i w znaczniku dla kontrolki `GridView` Określ procedurę obsługi dla zdarzenia `RowDataBound`. Znaczniki dla otwierającego tagu kontrolki są teraz podobne do poniższego przykładu.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

Dodaj nowy element `TemplateField` po polu szablonu `Administrator`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

Ten znacznik tworzy zagnieżdżony formant `GridView`, który pokazuje numer kursu i tytuł listy kursów. Nie określa źródła danych, ponieważ będzie on wiązać się z kodem w obsłudze `RowDataBound`.

Otwórz *Departments.aspx.cs* i Dodaj następujący program obsługi dla zdarzenia `RowDataBound`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

Ten kod pobiera `Department` jednostkę z argumentów zdarzenia, konwertuje właściwość nawigacji `Courses` do kolekcji `List`, a następnie datawiąże zagnieżdżoną `GridView` z kolekcją.

Otwórz plik *SchoolRepository.cs* i określ eager do załadowania dla właściwości nawigacji `Courses` przez wywołanie metody `Include` w zapytaniu obiektu utworzonym w metodzie `GetDepartmentsByName`. Instrukcja `return` w metodzie `GetDepartmentsByName` jest teraz podobna do poniższego przykładu.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

Uruchom stronę. Oprócz możliwości sortowania i filtrowania, które zostały dodane wcześniej, formant GridView pokazuje teraz zagnieżdżone szczegóły kursu dla każdego działu.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)

Spowoduje to zakończenie wprowadzania do sortowania, filtrowania i szczegółowych scenariuszy. W następnym samouczku zobaczysz, jak obsłużyć współbieżność.

> [!div class="step-by-step"]
> [Poprzednie](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
> [dalej](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)

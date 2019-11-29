---
uid: web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
title: Programowe Ustawianie wartości parametrów elementu ObjectDataSource (VB) | Microsoft Docs
author: rick-anderson
description: W tym samouczku dowiesz się, jak dodać metodę do naszych DAL i LOGIKI biznesowej, która akceptuje pojedynczy parametr wejściowy i zwraca dane. Przykład ustawi ten parametr...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 0ecb03b6-52a0-4731-8c7a-436391d36838
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
msc.type: authoredcontent
ms.openlocfilehash: f1dd50f46528e8dd51f85e503604d3f0dbc21ad2
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74601849"
---
# <a name="programmatically-setting-the-objectdatasources-parameter-values-vb"></a>Programowe ustawianie wartości parametrów elementu ObjectDataSource (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_6_VB.exe) lub [Pobierz plik PDF](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/datatutorial06vb1.pdf)

> W tym samouczku dowiesz się, jak dodać metodę do naszych DAL i LOGIKI biznesowej, która akceptuje pojedynczy parametr wejściowy i zwraca dane. Przykład ustawia ten parametr programowo.

## <a name="introduction"></a>Wprowadzenie

Jak zostało to opisane w [poprzednim samouczku](declarative-parameters-vb.md), dostępne są różne opcje umożliwiające deklaratywne przekazywanie wartości parametrów do metod elementu ObjectDataSource. Jeśli wartość parametru jest zakodowana na stałe, pochodzi z kontrolki sieci Web na stronie lub znajduje się w innym źródle, które jest odczytywane przez źródło danych `Parameter` obiektu, na przykład ta wartość może być powiązana z parametrem wejściowym bez pisania wiersza kodu.

Mogą jednak wystąpić sytuacje, gdy wartość parametru pochodzi z niektórych źródeł, które nie są jeszcze uwzględniane przez jedno z wbudowanych źródeł danych `Parameter` obiektów. Jeśli nasza witryna obsługuje konta użytkowników, warto ustawić parametr na podstawie identyfikatora użytkownika zalogowanego obecnie. Może być również konieczne dostosowanie wartości parametru przed wysłaniem go do metody obiektu bazowego elementu ObjectDataSource.

Za każdym razem, gdy metoda `Select` elementu ObjectDataSource jest wywoływana, element ObjectDataSource najpierw wywołuje jego [zdarzenie select](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting%28VS.80%29.aspx). Metoda obiektu bazowego elementu ObjectDataSource jest następnie wywoływana. Po zakończeniu tego procesu zostanie wyzwolone [wybrane zdarzenie](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selected%28VS.80%29.aspx) elementu ObjectDataSource (rysunek 1 ilustruje tę sekwencję zdarzeń). Wartości parametrów przesłane do metody obiektu bazowego elementu ObjectDataSource można ustawić lub dostosować w programie obsługi zdarzeń dla zdarzenia `Selecting`.

[![zaznaczonego elementu ObjectDataSource i wybierz zdarzenia wyzwalane przed i po wywołaniu metody obiektu źródłowego](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image2.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image1.png)

**Rysunek 1**: `Selected` i zdarzenia `Selecting` elementu ObjectDataSource przed wywołaniem i po wywołaniu metody obiektu źródłowego ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image3.png))

W tym samouczku dowiesz się, jak dodać metodę do naszych DAL i LOGIKI biznesowej, która akceptuje pojedynczy parametr wejściowy `Month`typu `Integer` i zwraca obiekt `EmployeesDataTable` wypełniony tym pracownikom, którzy mają rocznicę zatrudniania w określonym `Month`. Nasz przykład skonfiguruje ten parametr programowo na podstawie bieżącego miesiąca, pokazując listę "rocznice pracowników w tym miesiącu".

Zacznijmy!

## <a name="step-1-adding-a-method-toemployeestableadapter"></a>Krok 1. Dodawanie metody do`EmployeesTableAdapter`

Aby skorzystać z pierwszego przykładu, należy dodać metodę w celu pobrania tych pracowników, których `HireDate` wystąpiły w określonym miesiącu. Aby zapewnić tę funkcjonalność zgodnie z naszą architekturą, musimy najpierw utworzyć metodę w `EmployeesTableAdapter`, która mapuje do właściwej instrukcji SQL. Aby to osiągnąć, Zacznij od otworzenia zestawu danych Northwind. Kliknij prawym przyciskiem myszy etykietę `EmployeesTableAdapter` i wybierz polecenie Dodaj zapytanie.

[![dodać nowego zapytania do EmployeesTableAdapter](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image5.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image4.png)

**Rysunek 2**. Dodawanie nowego zapytania do `EmployeesTableAdapter` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image6.png))

Wybierz, aby dodać instrukcję SQL zwracającą wiersze. Gdy zostanie osiągnięty ekran "Określanie `SELECT` instrukcji", domyślna instrukcja `SELECT` dla `EmployeesTableAdapter` zostanie już załadowana. Po prostu Dodaj w klauzuli `WHERE`: `WHERE DATEPART(m, HireDate) = @Month`. [DatePart](https://msdn.microsoft.com/library/ms174420.aspx) jest funkcją T-SQL, która zwraca określoną część daty typu `datetime`; w tym przypadku używamy `DATEPART`, aby zwrócić miesiąc `HireDate`j kolumny.

[![zwracać tylko wiersze, w których kolumna HireDate jest mniejsza lub równa parametrowi @HiredBeforeDate](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image8.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image7.png)

**Rysunek 3**. Zwracanie tylko tych wierszy, w których kolumna `HireDate` jest mniejsza lub równa parametrowi `@HiredBeforeDate` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image9.png))

Na koniec Zmień nazwy metod `FillBy` i `GetDataBy` na odpowiednio `FillByHiredDateMonth` i `GetEmployeesByHiredDateMonth`.

[![wybrać bardziej odpowiednie nazwy metod niż FillBy i GetDataBy](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image11.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image10.png)

**Ilustracja 4**. Wybieranie bardziej odpowiednich nazw metod niż `FillBy` i `GetDataBy` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image12.png))

Kliknij przycisk Zakończ, aby zakończyć pracę kreatora i powrócić do powierzchni projektowej zestawu danych. `EmployeesTableAdapter` powinien teraz zawierać nowy zestaw metod do uzyskiwania dostępu do pracowników zatrudnionych w określonym miesiącu.

[![nowe metody są wyświetlane w Powierzchnia projektowa zestawu danych](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image14.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image13.png)

**Rysunek 5**. nowe metody są wyświetlane w powierzchnia projektowa zestawu danych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image15.png))

## <a name="step-2-adding-thegetemployeesbyhireddatemonthmonthmethod-to-the-business-logic-layer"></a>Krok 2. Dodawanie metody`GetEmployeesByHiredDateMonth(month)`do warstwy logiki biznesowej

Ponieważ nasza architektura aplikacji korzysta z oddzielnej warstwy dla logiki biznesowej i logiki dostępu do danych, musimy dodać metodę do LOGIKI biznesowej, która wywołuje DAL, aby pobrać pracowników zatrudnionych przed określoną datą. Otwórz plik `EmployeesBLL.vb` i Dodaj następującą metodę:

[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample1.vb)]

Podobnie jak w przypadku innych metod w tej klasie, `GetEmployeesByHiredDateMonth(month)` po prostu wywołuje do DAL i zwraca wyniki.

## <a name="step-3-displaying-employees-whose-hiring-anniversary-is-this-month"></a>Krok 3. Wyświetlanie pracowników, których rocznica zatrudnienia jest w tym miesiącu

Ostatnim krokiem tego przykładu jest wyświetlenie pracowników, których rocznica zatrudnienia jest w tym miesiącu. Zacznij od dodania widoku GridView do strony `ProgrammaticParams.aspx` w folderze `BasicReporting` i dodania nowego elementu ObjectDataSource jako źródła danych. Skonfiguruj element ObjectDataSource, aby używał klasy `EmployeesBLL` z `SelectMethod` ustawionym na `GetEmployeesByHiredDateMonth(month)`.

[![użyć klasy EmployeesBLL](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image17.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image16.png)

**Ilustracja 6**. użyj klasy `EmployeesBLL` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image18.png))

[![wybierać z metody GetEmployeesByHiredDateMonth (miesiąc)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image20.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image19.png)

**Rysunek 7**: wybór z metody `GetEmployeesByHiredDateMonth(month)` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image21.png))

Na ostatnim ekranie poprosimy nas o podanie wartości parametru `month`. Ponieważ ustawimy tę wartość programowo, pozostaw opcję Źródło parametru ustawioną Domyślnie none i kliknij przycisk Zakończ.

[![pozostawić Źródło parametru jako None](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image23.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image22.png)

**Ilustracja 8**. Pozostaw Źródło parametru ustawione na None ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image24.png))

Spowoduje to utworzenie obiektu `Parameter` w kolekcji `SelectParameters` elementu ObjectDataSource, która nie ma określonej wartości.

[!code-aspx[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample2.aspx)]

Aby programowo ustawić tę wartość, musimy utworzyć procedurę obsługi zdarzeń dla zdarzenia `Selecting` elementu ObjectDataSource. Aby to osiągnąć, przejdź do widok Projekt i kliknij dwukrotnie element ObjectDataSource. Alternatywnie wybierz element ObjectDataSource, przejdź do okno Właściwości, a następnie kliknij ikonę błyskawicy. Następnie kliknij dwukrotnie pole tekstowe obok zdarzenia `Selecting` lub wpisz nazwę programu obsługi zdarzeń, którego chcesz użyć. Jako trzecią opcję można utworzyć procedurę obsługi zdarzeń, wybierając element ObjectDataSource i jego zdarzenie `Selecting` z dwóch list rozwijanych w górnej części klasy z kodem związanym ze stroną.

![Kliknij ikonę pioruna w oknie właściwości, aby wyświetlić listę zdarzeń kontrolki sieci Web](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image25.png)

**Ilustracja 9**. Kliknij ikonę pioruna w oknie właściwości, aby wyświetlić listę zdarzeń kontrolki sieci Web

Wszystkie trzy podejścia dodają nowy program obsługi zdarzeń dla zdarzenia `Selecting` elementu ObjectDataSource do klasy powiązanej z kodem strony. W tym obsłudze zdarzeń możemy odczytywać i zapisywać wartości parametrów przy użyciu `e.InputParameters(parameterName)`, gdzie *`parameterName`* jest wartością atrybutu `Name` w tagu `<asp:Parameter>` (kolekcja `InputParameters` może być również indeksowana w `e.InputParameters(index)`). Aby ustawić parametr `month` na bieżący miesiąc, Dodaj następujący kod do programu obsługi zdarzeń `Selecting`:

[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample3.vb)]

Podczas odwiedzania tej strony za pośrednictwem przeglądarki można zobaczyć, że tylko jeden pracownik został zatrudniony w tym miesiącu (marzec) Laura Callahan, który jest członkiem firmy od 1994.

[![tym pracownikom, których rocznice w tym miesiącu są wyświetlane](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image27.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image26.png)

**Rysunek 10**. pracownicy, których rocznice w tym miesiącu są pokazywane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image28.png))

## <a name="summary"></a>Podsumowanie

Chociaż wartości parametrów elementu ObjectDataSource mogą być zazwyczaj ustawiane w sposób deklaratywny, bez konieczności korzystania z wiersza kodu, można łatwo ustawiać wartości parametrów programowo. Wystarczy utworzyć procedurę obsługi zdarzeń dla zdarzenia `Selecting` elementu ObjectDataSource, które zostanie wyzwolone przed wywołaniem metody obiektu źródłowego, i ręcznie ustawić wartości dla jednego lub kilku parametrów za pośrednictwem `InputParameters` kolekcji.

Ten samouczek zawiera podstawową sekcję raportowania. W [następnym samouczku](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) znajdują się sekcje filtrowanie i szczegółowe informacje o scenariuszach, w których zawarto informacje o metodach umożliwiających odwiedzającemu odfiltrowanie danych i przechodzenie do szczegółów raportu głównego.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzent potencjalnych klientów dla tego samouczka został Hilton Giesenow. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Ubiegł](declarative-parameters-vb.md)

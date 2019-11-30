---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-vb
title: Używanie używanie TemplateField w formancie GridView (VB) | Microsoft Docs
author: rick-anderson
description: Aby zapewnić elastyczność, widok GridView oferuje TemplateField, który renderuje przy użyciu szablonu. Szablon może zawierać kombinację statycznych HTML, kontrolek sieci Web i...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: a92cd6ed-609a-4e40-ad23-004b54afd436
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 3c090dbf65d9acbcc0e343cda5e8da7fff2d35d3
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74581511"
---
# <a name="using-templatefields-in-the-gridview-control-vb"></a>Używanie kontrolek TemplateField w kontrolce GridView (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_12_VB.exe) lub [Pobierz plik PDF](using-templatefields-in-the-gridview-control-vb/_static/datatutorial12vb1.pdf)

> Aby zapewnić elastyczność, widok GridView oferuje TemplateField, który renderuje przy użyciu szablonu. Szablon może zawierać kombinację statycznych HTML, kontrolek sieci Web i składni wiązania danych. W tym samouczku sprawdzimy, jak używać TemplateField, aby osiągnąć większy stopień dostosowywania za pomocą kontrolki GridView.

## <a name="introduction"></a>Wprowadzenie

Widok GridView składa się z zestawu pól wskazujących właściwości `DataSource`, które mają być uwzględnione w renderowanych danych wyjściowych, oraz sposób wyświetlania danych. Najprostszym typem pola jest BoundField, który wyświetla wartość danych jako tekst. Inne typy pól wyświetlają dane przy użyciu alternatywnych elementów HTML. CheckBoxField, na przykład, renderuje jako pole wyboru, którego stan zaznaczono, zależy od wartości określonego pola danych; ImageField renderuje obraz, którego źródło obrazu jest oparte na określonym polu danych. Hiperłącza i przyciski, których stan zależy od wartości pola danych źródłowych, mogą być renderowane przy użyciu typów pól HyperLinkField i ButtonField.

Chociaż typy pól CheckBoxField, ImageField, HyperLinkField i ButtonField umożliwiają użycie alternatywnego widoku danych, nadal są one dość ograniczone w odniesieniu do formatowania. CheckBoxField może wyświetlić tylko jedno pole wyboru, podczas gdy ImageField może wyświetlać tylko jeden obraz. Co w przypadku, gdy określone pole musi wyświetlać jakiś tekst, pole wyboru *i* obraz, na podstawie różnych wartości pól danych? Lub czego potrzebujesz, aby wyświetlić dane przy użyciu kontrolki sieci Web innego niż pole wyboru, obraz, hiperłącze lub przycisk? Ponadto BoundField ogranicza jego wyświetlanie do jednego pola danych. Co zrobić, jeśli chcemy pokazać dwie lub więcej wartości pól danych w pojedynczej kolumnie GridView?

Aby obsłużyć ten poziom elastyczności, widok GridView oferuje TemplateField, który renderuje przy użyciu *szablonu*. Szablon może zawierać kombinację statycznych HTML, kontrolek sieci Web i składni wiązania danych. Ponadto TemplateField ma wiele szablonów, których można użyć do dostosowania renderowania w różnych sytuacjach. Na przykład `ItemTemplate` jest używany domyślnie do renderowania komórki dla każdego wiersza, ale szablon `EditItemTemplate` może służyć do dostosowywania interfejsu podczas edytowania danych.

W tym samouczku sprawdzimy, jak używać TemplateField, aby osiągnąć większy stopień dostosowywania za pomocą kontrolki GridView. W [poprzednim samouczku](custom-formatting-based-upon-data-vb.md) pokazano, jak dostosować formatowanie na podstawie danych źródłowych przy użyciu `DataBound` i `RowDataBound` obsługi zdarzeń. Innym sposobem dostosowania formatowania na podstawie danych źródłowych jest wywołanie metod formatowania z poziomu szablonu. W tym samouczku Przyjrzyjmy się tej metodzie.

W tym samouczku będziemy używać używanie TemplateField, aby dostosować wygląd listy pracowników. W każdym przypadku zostanie wyświetlona lista wszystkich pracowników, ale w jednej kolumnie pojawią się imiona i nazwiska pracownika oraz kolumna stan, która wskazuje liczbę dni, w których zostały zatrudnione w firmie.

[![trzy używanie TemplateField są używane do dostosowywania wyświetlania](using-templatefields-in-the-gridview-control-vb/_static/image2.png)](using-templatefields-in-the-gridview-control-vb/_static/image1.png)

**Rysunek 1**: trzy używanie TemplateField są używane do dostosowywania wyświetlania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-gridview-control-vb/_static/image3.png))

## <a name="step-1-binding-the-data-to-the-gridview"></a>Krok 1. Powiązywanie danych z GridView

W scenariuszach raportowania, w których należy użyć używanie TemplateField, aby dostosować wygląd, najłatwiej zacząć od utworzenia kontrolki GridView, która zawiera tylko BoundFields najpierw, a następnie dodać nową używanie TemplateField lub przekonwertować istniejącą BoundFields do Używanie TemplateField zgodnie z wymaganiami. W związku z tym Uruchommy ten samouczek, dodając widok GridView do strony za pomocą projektanta i powiąż go z elementem ObjectDataSource, który zwraca listę pracowników. Te kroki spowodują utworzenie widoku GridView z BoundFields dla każdego z pól pracowników.

Otwórz stronę `GridViewTemplateField.aspx` i przeciągnij widok GridView z przybornika do projektanta. Z tagu inteligentnego GridView wybierz, aby dodać nową kontrolkę ObjectDataSource, która wywołuje metodę `GetEmployees()` klasy `EmployeesBLL`.

[![dodać nowej kontrolki ObjectDataSource, która wywołuje metodę GetEmployees ()](using-templatefields-in-the-gridview-control-vb/_static/image5.png)](using-templatefields-in-the-gridview-control-vb/_static/image4.png)

**Rysunek 2**. Dodawanie nowej kontrolki ObjectDataSource, która wywołuje metodę `GetEmployees()` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-gridview-control-vb/_static/image6.png))

Powiązanie widoku GridView w ten sposób spowoduje automatyczne dodanie BoundField dla każdej właściwości pracownika: `EmployeeID`, `LastName`, `FirstName`, `Title`, `HireDate`, `ReportsTo`i `Country`. Dla tego raportu nie bother się z wyświetlaniem właściwości `EmployeeID`, `ReportsTo`lub `Country`. Aby usunąć te BoundFields, możesz:

- Skorzystaj z okna dialogowego pola, a następnie kliknij link Edytuj kolumny w tagu inteligentnym GridView, aby wyświetlić to okno dialogowe. Następnie wybierz BoundFields z listy dolnej po lewej stronie, a następnie kliknij przycisk czerwony X, aby usunąć BoundField.
- Edytuj ręcznie składnię deklaratywną GridView z widoku źródła, Usuń element `<asp:BoundField>` dla BoundField, który chcesz usunąć.

Po usunięciu `EmployeeID`, `ReportsTo`i `Country` BoundFields, znacznik GridView powinien wyglądać następująco:

[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample1.aspx)]

Poświęć chwilę na wyświetlenie postępu w przeglądarce. W tym momencie powinna zostać wyświetlona tabela z rekordem dla każdego pracownika i czterech kolumn: jeden dla nazwiska pracownika, po jednym dla swojej pierwszej nazwy, po jednym dla swojego tytułu, a drugi dla daty zatrudnienia.

[![pola LastName, FirstName, title i HireDate są wyświetlane dla każdego pracownika](using-templatefields-in-the-gridview-control-vb/_static/image8.png)](using-templatefields-in-the-gridview-control-vb/_static/image7.png)

**Rysunek 3**. pola `LastName`, `FirstName`, `Title`i `HireDate` są wyświetlane dla każdego pracownika ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-gridview-control-vb/_static/image9.png))

## <a name="step-2-displaying-the-first-and-last-names-in-a-single-column"></a>Krok 2. Wyświetlanie imion i nazwisk w jednej kolumnie

Obecnie każdy pracownik i nazwisko są wyświetlane w oddzielnej kolumnie. W zamian warto połączyć je w jedną kolumnę. Aby to osiągnąć, należy użyć TemplateField. Możemy dodać nowy TemplateField, dodać do niego wymaganą składnię znaczników i powiązania, a następnie usunąć `FirstName` i `LastName` BoundFields, lub przekonwertować `FirstName` BoundField na TemplateField, edytować TemplateField w celu uwzględnienia wartości `LastName`, a następnie usunąć `LastName` BoundField.

Obie podejścia powodują ten sam wynik, ale w przypadku, gdy jest to możliwe, należy wykonać konwersję BoundFields na używanie TemplateField, ponieważ konwersja automatycznie dodaje `ItemTemplate` i `EditItemTemplate` za pomocą kontrolek sieci Web i składni wiązania danych, aby naśladować wygląd i funkcjonalność BoundField. Korzyścią jest, że konieczna będzie mniejsza współpraca z TemplateField, ponieważ proces konwersji wykona część pracy.

Aby skonwertować istniejący BoundField na TemplateField, kliknij link Edytuj kolumny w tagu inteligentnym GridView, wyłączając okno dialogowe pola. Wybierz BoundField do przekonwertowania z listy w lewym dolnym rogu, a następnie kliknij link "Konwertuj to pole na TemplateField" w prawym dolnym rogu.

[![skonwertować BoundField na TemplateField z okna dialogowego pola](using-templatefields-in-the-gridview-control-vb/_static/image11.png)](using-templatefields-in-the-gridview-control-vb/_static/image10.png)

**Ilustracja 4**. przekonwertowanie BoundField na TemplateField z okna dialogowego pola ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-templatefields-in-the-gridview-control-vb/_static/image12.png))

Przejdź dalej i Konwertuj `FirstName` BoundField na TemplateField. Po tej zmianie nie ma różnicy Perceptive w projektancie. Dzieje się tak, ponieważ konwersja BoundField na TemplateField Tworzy TemplateField, który utrzymuje wygląd i działanie BoundField. Pomimo tego, że w tym momencie nie ma żadnej różnicy wizualnej, ten proces konwersji zamienił składnię deklaratywną BoundField-`<asp:BoundField DataField="FirstName" HeaderText="FirstName" SortExpression="FirstName" />`-z następującą składnią TemplateField:

[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample2.aspx)]

Jak widać, TemplateField składa się z dwóch szablonów `ItemTemplate`, które mają etykietę, której Właściwość `Text` jest ustawiona na wartość pola dane `FirstName` i `EditItemTemplate` z kontrolką TextBox, której Właściwość `Text` jest również ustawiona na pole danych `FirstName`. Składnia DataBinding-`<%# Bind("fieldName") %>`-wskazuje, że pole danych *`fieldName`* jest powiązane z określoną właściwością kontrolki sieci Web.

Aby dodać wartość pola `LastName` danych do tej TemplateField musimy dodać kolejną kontrolkę sieci Web etykiety w `ItemTemplate` i powiązać jej Właściwość `Text` z `LastName`. Można to zrobić ręcznie lub za pomocą projektanta. Aby to zrobić, wystarczy dodać do `ItemTemplate`odpowiednią składnię deklaratywną:

[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample3.aspx)]

Aby dodać go za pomocą projektanta, kliknij link Edytuj szablony w tagu inteligentnym GridView. Spowoduje to wyświetlenie interfejsu edytowania szablonu GridView. Tag inteligentny tego interfejsu jest listą szablonów w widoku GridView. Ponieważ w tym momencie mamy tylko jeden TemplateField, jedynymi szablonami wymienionymi na liście rozwijanej są te szablony dla `FirstName` TemplateField, a także `EmptyDataTemplate` i `PagerTemplate`. Szablon `EmptyDataTemplate`, jeśli jest określony, służy do renderowania danych wyjściowych GridView, jeśli nie ma wyników związanych z danymi w widoku GridView; `PagerTemplate`, jeśli jest określony, służy do renderowania interfejsu stronicowania dla widoku GridView, który obsługuje stronicowanie.

[![szablonów GridView można edytować za pomocą narzędzia Projektant](using-templatefields-in-the-gridview-control-vb/_static/image14.png)](using-templatefields-in-the-gridview-control-vb/_static/image13.png)

**Rysunek 5**. szablony programu GridView można edytować za pomocą projektanta ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-templatefields-in-the-gridview-control-vb/_static/image15.png))

Aby również wyświetlić `LastName` w `FirstName` TemplateField, przeciągnij kontrolkę etykieta z przybornika do `ItemTemplate` `FirstName` TemplateField w interfejsie edycji szablonu GridView.

[![dodać kontrolkę sieci Web etykieta do ItemTemplatea FirstName TemplateField](using-templatefields-in-the-gridview-control-vb/_static/image17.png)](using-templatefields-in-the-gridview-control-vb/_static/image16.png)

**Ilustracja 6**. Dodawanie kontrolki sieci Web etykieta do ItemTemplate `FirstName` TemplateField ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-gridview-control-vb/_static/image18.png))

W tym momencie kontrolka sieci Web etykieta dodana do TemplateField ma właściwość `Text` ustawioną na wartość "etykieta". Musimy zmienić to pole, tak aby ta właściwość była powiązana z wartością pola danych `LastName`. Aby to zrobić, kliknij tag inteligentny kontrolki etykieta i wybierz opcję Edytuj powiązania danych.

[![wybrać opcji Edytuj powiązania danych z tagu inteligentnego etykiety](using-templatefields-in-the-gridview-control-vb/_static/image20.png)](using-templatefields-in-the-gridview-control-vb/_static/image19.png)

**Rysunek 7**. Wybierz opcję Edytuj powiązania danych z tagu inteligentnego etykiety ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-templatefields-in-the-gridview-control-vb/_static/image21.png))

Spowoduje to wyświetlenie okna dialogowego DataBindings. W tym miejscu możesz wybrać właściwość, która będzie uczestniczyć w zbieraniu danych z listy po lewej stronie, a następnie wybrać pole, z którym mają być powiązane dane, z listy rozwijanej po prawej stronie. Wybierz właściwość `Text` po lewej stronie, a pole `LastName` z prawej strony i kliknij przycisk OK.

[![powiązać właściwość Text z polem LastName Data](using-templatefields-in-the-gridview-control-vb/_static/image23.png)](using-templatefields-in-the-gridview-control-vb/_static/image22.png)

**Ilustracja 8**. powiązanie właściwości `Text` z polem danych `LastName` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-templatefields-in-the-gridview-control-vb/_static/image24.png))

> [!NOTE]
> Okno dialogowe DataBindings umożliwia wskazanie, czy ma być wykonywane dwukierunkowa DataBinding. Jeśli pozostawisz tę niesprawdzoną, zamiast `<%# Bind("LastName")%>`zostanie użyta składnia wiązania danych `<%# Eval("LastName")%>`. Jedno z tych rozwiązań jest dla tego samouczka. Dwukierunkowa DataBinding jest ważna podczas wstawiania i edytowania danych. Jednak w przypadku zwykłego wyświetlania danych jedno z tych rozwiązań będzie działało równie dobrze. W następnych samouczkach szczegółowo omówiono dwukierunkowe powiązanie danych.

Poświęć chwilę na wyświetlenie tej strony za pomocą przeglądarki. Jak widać, widok GridView nadal zawiera cztery kolumny; jednak kolumna `FirstName` *teraz zawiera wartości* pól danych `FirstName` i `LastName`.

[![wartości FirstName i LastName są pokazane w jednej kolumnie.](using-templatefields-in-the-gridview-control-vb/_static/image26.png)](using-templatefields-in-the-gridview-control-vb/_static/image25.png)

**Rysunek 9**. wartości `FirstName` i `LastName` są wyświetlane w pojedynczej kolumnie ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-templatefields-in-the-gridview-control-vb/_static/image27.png))

Aby wykonać ten pierwszy krok, Usuń `LastName` BoundField i Zmień nazwę właściwości `HeaderText` `FirstName` TemplateField na "name". Po wprowadzeniu zmian znaczniki deklaratywne GridView powinny wyglądać następująco:

[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample4.aspx)]

[![imiona i nazwiska poszczególnych pracowników są wyświetlane w jednej kolumnie](using-templatefields-in-the-gridview-control-vb/_static/image29.png)](using-templatefields-in-the-gridview-control-vb/_static/image28.png)

**Ilustracja 10**. imiona i nazwiska każdego pracownika są wyświetlane w jednej kolumnie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-gridview-control-vb/_static/image30.png))

## <a name="step-3-using-the-calendar-control-to-display-thehireddatefield"></a>Krok 3. Używanie formantu Calendar do wyświetlania pola`HiredDate`

Wyświetlanie wartości pola danych jako tekstu w widoku GridView jest tak proste jak przy użyciu BoundField. W niektórych scenariuszach dane są jednak najlepiej wyrażone przy użyciu określonego formantu sieci Web, a nie tylko tekstu. Takie dostosowanie wyświetlania danych jest możliwe za pomocą używanie TemplateField. Na przykład zamiast wyświetlać datę zatrudnienia pracownika jako tekst, można wyświetlić kalendarz (przy użyciu [kontrolki Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar(VS.80).aspx)) z wyróżnioną datą zatrudnienia.

Aby to osiągnąć, Zacznij od przekonwertowania `HiredDate` BoundField na TemplateField. Po prostu przejdź do tagu inteligentnego GridView i kliknij łącze Edytuj kolumny, łącząc okno dialogowe pola. Wybierz `HiredDate` BoundField i kliknij pozycję "Konwertuj to pole na TemplateField".

[![skonwertować BoundField HiredDate na TemplateField](using-templatefields-in-the-gridview-control-vb/_static/image32.png)](using-templatefields-in-the-gridview-control-vb/_static/image31.png)

**Ilustracja 11**. konwertowanie `HiredDate` BoundField na TemplateField ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-gridview-control-vb/_static/image33.png))

Jak zostało to opisane w kroku 2, spowoduje to zastąpienie BoundField z TemplateField, który zawiera `ItemTemplate` i `EditItemTemplate` z etykietą i polem tekstowym, których właściwości `Text` są powiązane z wartością `HiredDate` przy użyciu składni wiązania danych `<%# Bind("HiredDate")%>`.

Aby zastąpić tekst kontrolką kalendarza, Edytuj szablon, usuwając etykietę i dodając kontrolkę Calendar. Z poziomu projektanta wybierz pozycję Edytuj szablony z tagu inteligentnego GridView i wybierz `ItemTemplate` `HireDate` TemplateField z listy rozwijanej. Następnie usuń kontrolkę etykieta i przeciągnij kontrolkę Calendar z przybornika do interfejsu edycji szablonu.

[![dodać kontrolkę Calendar do ItemTemplate TemplateField HireDate](using-templatefields-in-the-gridview-control-vb/_static/image35.png)](using-templatefields-in-the-gridview-control-vb/_static/image34.png)

**Ilustracja 12**. Dodawanie kontrolki Calendar do `ItemTemplate` `HireDate` TemplateField ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-gridview-control-vb/_static/image36.png))

W tym momencie każdy wiersz w widoku GridView będzie zawierał kontrolkę Calendar w `HiredDate` TemplateField. Jednak rzeczywista wartość `HiredDate` pracownika nie jest ustawiona w żadnym miejscu kontrolki Calendar, co powoduje, że każda kontrolka Calendar domyślnie wyświetla bieżący miesiąc i datę. Aby rozwiązać ten konieczność, musimy przypisać `HiredDate` każdego pracownika do właściwości [SelectedDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selecteddate(VS.80).aspx) i [VisibleDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.visibledate(VS.80).aspx) formantu Calendar.

Z tagu inteligentnego kontrolki Calendar wybierz pozycję Edytuj powiązania danych. Następnie powiąż właściwości `SelectedDate` i `VisibleDate` z polem danych `HiredDate`.

[![powiązać właściwości SelectedDate i VisibleDate z polem danych HiredDate](using-templatefields-in-the-gridview-control-vb/_static/image38.png)](using-templatefields-in-the-gridview-control-vb/_static/image37.png)

**Ilustracja 13**. powiązanie właściwości `SelectedDate` i `VisibleDate` z polem danych `HiredDate` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-templatefields-in-the-gridview-control-vb/_static/image39.png))

> [!NOTE]
> Wybrana data formantu kalendarza nie musi być widoczna. Na przykład kalendarz może mieć 1 sierpnia, 1999 jako wybraną<sup>datę, ale</sup>powinien wyświetlać bieżący miesiąc i rok. Wybrana data i Data widoczna są określane przez `SelectedDate` i właściwości `VisibleDate` formantu Calendar. Ponieważ chcemy wybrać `HiredDate` pracownika i upewnić się, że jest on widoczny, należy powiązać obie te właściwości z polem danych `HireDate`.

Podczas wyświetlania strony w przeglądarce w kalendarzu jest teraz wyświetlany miesiąc daty zatrudnienia pracownika i wybierana jest określona data.

[![HiredDate pracownika jest pokazywana w kontrolce Calendar](using-templatefields-in-the-gridview-control-vb/_static/image41.png)](using-templatefields-in-the-gridview-control-vb/_static/image40.png)

**Ilustracja 14**. `HiredDate` pracownika jest pokazywana w kontrolce Calendar ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-templatefields-in-the-gridview-control-vb/_static/image42.png))

> [!NOTE]
> W przeciwieństwie do wszystkich przykładów, które już się zaobserwowano, w tym samouczku *nie* została ustawiona właściwość `EnableViewState` na `False` dla tego widoku GridView. Przyczyną tej decyzji jest fakt, że kliknięcie dat w kontrolce Calendar powoduje odświeżenie, ustawienie wybranej daty w kalendarzu na klikniętą datę. Jeśli stan widoku GridView jest wyłączony, jednak na każdym ogłaszaniu zwrotnym dane GridView są przełączane do jego bazowego źródła danych, co powoduje, że wybrana data zostanie ustawiona z *powrotem* do `HireDate`pracownika, zastępując datę wybraną przez użytkownika.

W tym samouczku jest to moota dyskusja, ponieważ użytkownik nie może zaktualizować `HireDate`pracownika. Prawdopodobnie najlepiej jest skonfigurować kontrolkę kalendarz, aby nie można było wybierać jej dat. Bez względu na ten samouczek pokazuje, że w niektórych okolicznościach stan widoku musi być włączony, aby można było zapewnić pewne funkcje.

## <a name="step-4-showing-the-number-of-days-the-employee-has-worked-for-the-company"></a>Krok 4. pokazuje liczbę dni, przez które pracownik pracował dla firmy

Do tej pory zaobserwowano dwie aplikacje używanie TemplateField:

- Łączenie dwóch lub więcej wartości pól danych w jedną kolumnę, i
- Wyrażanie wartości pola danych przy użyciu kontrolki sieci Web zamiast tekstu

Trzecie użycie używanie TemplateField polega na wyświetlaniu metadanych na temat danych bazowych GridView. Oprócz wyświetlania dat zatrudnienia pracowników, na przykład może być również potrzebna kolumna, która wyświetla liczbę dni, które były w zadaniu.

Jeszcze inne użycie używanie TemplateField pojawia się w scenariuszach, gdy dane podstawowe muszą być wyświetlane inaczej w raporcie na stronie sieci Web niż w formacie, który jest przechowywany w bazie danych. Załóżmy, że tabela `Employees` miała `Gender` pole, które przechowuje znak `M` lub `F`, aby wskazać płeć pracownika. Gdy te informacje są wyświetlane na stronie sieci Web, firma Microsoft może chcieć pokazać płeć jako "męski" lub "kobieta", a nie tylko "M" lub "F".

Oba te scenariusze mogą być obsługiwane przez utworzenie *metody formatowania* w klasie powiązanej z kodem ASP.NET strony (lub w oddzielnej bibliotece klas wdrożonej jako metoda `Shared`), która jest wywoływana z szablonu. Taka metoda formatowania jest wywoływana z szablonu za pomocą tej samej składni wiązania danych, która była widoczna wcześniej. Metoda formatowania może przyjmować dowolną liczbę parametrów, ale musi zwracać ciąg. Zwrócony ciąg to kod HTML, który jest wstrzykiwany do szablonu.

Aby zilustrować ten pomysł, przyjrzyjmy naszym samouczkowi, aby wyświetlić kolumnę zawierającą listę łącznej liczby dni, w których pracownik przekazał zadanie. Ta metoda formatowania zajmie `Northwind.EmployeesRow` obiektu i zwróci liczbę dni, przez które pracownik został zatrudniony jako ciąg. Tę metodę można dodać do klasy związanej z kodem ASP.NET strony, ale *musi* ona być oznaczona jako `Protected` lub `Public`, aby była dostępna z szablonu.

[!code-vb[Main](using-templatefields-in-the-gridview-control-vb/samples/sample5.vb)]

Ponieważ `HiredDate` pole może zawierać `NULL` wartości bazy danych, należy najpierw upewnić się, że wartość nie jest `NULL` przed kontynuowaniem obliczania. Jeśli wartość `HiredDate` jest `NULL`, po prostu zwracamy ciąg "nieznany"; Jeśli nie `NULL`, obliczy różnicę między bieżącą godziną a wartością `HiredDate` i zwróci liczbę dni.

Aby użyć tej metody, należy wywołać ją z TemplateField w widoku GridView przy użyciu składni wiązania danych. Zacznij od dodania nowego TemplateField do widoku GridView, klikając link Edytuj kolumny w tagu inteligentnym GridView i dodając nowy TemplateField.

[![dodać nowego TemplateField do widoku GridView](using-templatefields-in-the-gridview-control-vb/_static/image44.png)](using-templatefields-in-the-gridview-control-vb/_static/image43.png)

**Ilustracja 15**. Dodawanie nowego TemplateField do widoku GridView ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-templatefields-in-the-gridview-control-vb/_static/image45.png))

Ustaw tę nową właściwość `HeaderText` TemplateField na "dni w zadaniu" i jej Właściwość `HorizontalAlign` `ItemStyle`na `Center`. Aby wywołać metodę `DisplayDaysOnJob` z szablonu, Dodaj `ItemTemplate` i użyj następującej składni wiązania z danymi:

[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample6.aspx)]

`Container.DataItem` zwraca obiekt `DataRowView` odpowiadający rekordowi `DataSource` powiązanemu z `GridViewRow`. Jej Właściwość `Row` zwraca `Northwind.EmployeesRow`o jednoznacznie określonym typie, który jest przesyłany do metody `DisplayDaysOnJob`. Ta składnia wiązania danych może występować bezpośrednio w `ItemTemplate` (jak pokazano w składni deklaracyjnej poniżej) lub można ją przypisać do właściwości `Text` kontrolki sieci Web etykieta.

> [!NOTE]
> Alternatywnie, zamiast przekazywać wystąpienie `EmployeesRow`, można po prostu przekazać wartość `HireDate` przy użyciu `<%# DisplayDaysOnJob(Eval("HireDate")) %>`. Jednak Metoda `Eval` zwraca `Object`, dlatego będziemy musieli zmienić sygnaturę metody `DisplayDaysOnJob` w celu zaakceptowania parametru wejściowego typu `Object`, zamiast tego. Nie możemy oddzwonić wywołania `Eval("HireDate")` do `DateTime`, ponieważ kolumna `HireDate` w tabeli `Employees` może zawierać wartości `NULL`. W związku z tym musimy przyjąć `Object` jako parametr wejściowy metody `DisplayDaysOnJob`, sprawdzić, czy ma ona `NULL` wartość bazy danych (którą można wykonać przy użyciu `Convert.IsDBNull(objectToCheck)`), a następnie wykonać odpowiednie czynności.

Ze względu na te subtleties zostały wybrane do przekazania w całym wystąpieniu `EmployeesRow`. W następnym samouczku zobaczymy dokładniejszy przykład użycia składni `Eval("columnName")` do przekazywania parametru wejściowego do metody formatowania.

Poniżej przedstawiono składnię deklaratywną dla naszego widoku GridView po dodaniu TemplateField oraz metodę `DisplayDaysOnJob` wywołana z `ItemTemplate`:

[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample7.aspx)]

Rysunek 16 pokazuje ukończony samouczek, gdy jest wyświetlany za pomocą przeglądarki.

[![liczba dni, w których zostanie wyświetlony pracownik zadania](using-templatefields-in-the-gridview-control-vb/_static/image47.png)](using-templatefields-in-the-gridview-control-vb/_static/image46.png)

**Rysunek 16**: liczba dni, przez które pracownik przeszedł do zadania ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-templatefields-in-the-gridview-control-vb/_static/image48.png))

## <a name="summary"></a>Podsumowanie

TemplateField w formancie GridView umożliwia wyższy poziom elastyczności wyświetlania danych niż jest dostępny z innymi kontrolkami pól. Używanie TemplateField doskonale nadaje się do sytuacji, w których:

- W jednej kolumnie GridView muszą być wyświetlone wiele pól danych
- Dane są najlepiej wyrażone przy użyciu kontrolki sieci Web, a nie zwykłego tekstu.
- Dane wyjściowe są zależne od danych źródłowych, takich jak wyświetlanie metadanych lub ponowne formatowanie danych

Oprócz dostosowywania wyświetlania danych używanie TemplateField są również używane do dostosowywania interfejsów użytkownika używanych do edycji i wstawiania danych, jak widać w przyszłych samouczkach.

Następne dwa samouczki kontynuują Eksplorowanie szablonów, rozpoczynając od użycia używanie TemplateField w widoku DetailsView. Poniżej przywrócimy do FormView, który używa szablonów w miejsce pól, aby zapewnić większą elastyczność układu i struktury danych.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Osoba dokonująca przeglądu potencjalnego klienta dla tego samouczka miała wartość Dan Jagers. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](custom-formatting-based-upon-data-vb.md)
> [dalej](using-templatefields-in-the-detailsview-control-vb.md)

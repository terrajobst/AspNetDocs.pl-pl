---
uid: web-forms/overview/moving-to-aspnet-20/data-bound-controls
title: Formanty powiązane z danymi | Microsoft Docs
author: microsoft
description: Większość aplikacji ASP.NET opiera się na pewnym stopniu prezentacji danych ze źródła danych zaplecza. Formanty powiązane z danymi były częścią obrotową działania w...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 0e23ff32-646d-43f3-8bec-6b2313d3abd6
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-bound-controls
msc.type: authoredcontent
ms.openlocfilehash: 1154b38e0fa3d9d56cb407ae659c3b0d69871fda
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78603839"
---
# <a name="data-bound-controls"></a>Kontrolki powiązania danych

przez [firmę Microsoft](https://github.com/microsoft)

> Większość aplikacji ASP.NET opiera się na pewnym stopniu prezentacji danych ze źródła danych zaplecza. Formanty powiązane z danymi były częścią wychodzącą z danych w dynamicznych aplikacjach sieci Web. ASP.NET 2,0 wprowadza istotne ulepszenia formantów związanych z danymi, w tym nową klasę BaseDataBoundControl i składnię deklaratywną.

Większość aplikacji ASP.NET opiera się na pewnym stopniu prezentacji danych ze źródła danych zaplecza. Formanty powiązane z danymi były częścią wychodzącą z danych w dynamicznych aplikacjach sieci Web. ASP.NET 2,0 wprowadza istotne ulepszenia formantów związanych z danymi, w tym nową klasę BaseDataBoundControl i składnię deklaratywną.

BaseDataBoundControl działa jako klasa bazowa dla klasy obiekt DataBoundControl i klasy HierarchicalDataBoundControl. W tym module będziemy omawiać następujące klasy, które pochodzą od obiekt DataBoundControl:

- AdRotator
- Kontrolki listy
- GridView
- FormView
- DetailsView

Omawiamy również następujące klasy, które pochodzą z klasy HierarchicalDataBoundControl:

- TreeView
- Menu
- SiteMapPath

## <a name="databoundcontrol-class"></a>Klasa obiekt DataBoundControl

Klasa obiekt DataBoundControl jest klasą abstrakcyjną (oznaczoną jako MustInherit w języku VB) służącą do korzystania z danych tabelarycznych lub w stylu listy. Następujące kontrolki są częścią formantów, które pochodzą od obiekt DataBoundControl.

## <a name="adrotator"></a>AdRotator

Formant AdRotator umożliwia wyświetlenie transparentu graficznego na stronie sieci Web, która jest połączona z określonym adresem URL. Wyświetlana ilustracja jest obracana przy użyciu właściwości formantu. Częstotliwość określonej usługi AD wyświetlanej na stronie można skonfigurować przy użyciu właściwości **wrażenia** i reklamy można filtrować przy użyciu funkcji filtrowania słów kluczowych.

Formanty AdRotator używają pliku XML lub tabeli w bazie danych w celu uzyskania danych. Następujące atrybuty są używane w plikach XML do konfigurowania formantu AdRotator.

### <a name="imageurl"></a>ImageUrl
Adres URL obrazu, który ma być wyświetlany dla usługi AD.

### <a name="navigateurl"></a>NavigateUrl
Adres URL, do którego użytkownik powinien zostać pobrany po kliknięciu usługi AD. Powinno to być kodowanie adresu URL.

### <a name="alternatetext"></a>AlternateText
Tekst alternatywny, który jest wyświetlany w etykietce narzędzia i odczytywany przez czytniki zawartości ekranu. Jest również wyświetlany, gdy obraz określony przez ImageUrl jest niedostępny.

### <a name="keyword"></a>Słowo kluczowe
Definiuje słowo kluczowe, które może być używane w przypadku filtrowania słowa kluczowego. Jeśli ta wartość jest określona, zostanie wyświetlonych tylko te reklamy, które pasują do filtru słów kluczowych.

### <a name="impressions"></a>Odcisk
Liczba ważona, która określa, jak często może się pojawić określona reklama. Jest to względne dla wrażenia innych reklam w tym samym pliku. Maksymalna wartość zbiorowych danych dla wszystkich reklam w pliku XML to 2 048 000 000 1.

### <a name="height"></a>Wysokość
Wysokość usługi AD w pikselach.

### <a name="width"></a>impulsów
Szerokość reklamy w pikselach.

> [!NOTE]
> Atrybuty Height i Width przesłaniają wysokość i szerokość formantu AdRotator.

Typowy plik XML może wyglądać następująco:

[!code-xml[Main](data-bound-controls/samples/sample1.xml)]

W powyższym przykładzie usługa AD dla contoso jest dwa razy widoczna jako AD dla witryny sieci Web ASP.NET z powodu wartości atrybutu wrażenia.

Aby wyświetlić reklamy z powyższego pliku XML, Dodaj formant AdRotator do strony i ustaw właściwość **AdvertisementFile** tak, aby wskazywała plik XML, jak pokazano poniżej:

[!code-aspx[Main](data-bound-controls/samples/sample2.aspx)]

Jeśli zdecydujesz się użyć tabeli bazy danych jako źródła danych dla formantu AdRotator, najpierw musisz skonfigurować bazę danych przy użyciu następującego schematu:

| **Nazwa kolumny** | **Typ danych** | **Opis** |
| --- | --- | --- |
| ID | int | Klucz podstawowy. Ta kolumna może zawierać dowolną nazwę. |
| ImageUrl | nvarchar (*Długość*) | Względny lub bezwzględny adres URL obrazu do wyświetlenia dla usługi AD. |
| NavigateUrl | nvarchar (*Długość*) | Docelowy adres URL dla usługi AD. Jeśli nie podasz wartości, AD nie jest hiperłączem. |
| AlternateText | nvarchar (*Długość*) | Tekst wyświetlany, jeśli nie można znaleźć obrazu. W niektórych przeglądarkach tekst jest wyświetlany jako etykietka narzędzia. Tekst alternatywny jest również używany na potrzeby ułatwień dostępu, dzięki czemu użytkownicy, którzy nie widzą grafiki, mogą usłyszeć swój opis odczytywać głośnie. |
| Słowo kluczowe | nvarchar (*Długość*) | Kategoria dla usługi AD, w której można filtrować stronę. |
| Odcisk | int (4) | Liczba, która wskazuje na prawdopodobieństwo, jak często jest wyświetlana AD. Im większa liczba, tym częściej zostanie wyświetlona wartość AD. Suma wszystkich wartości parametrów w pliku XML nie może przekraczać 2 048 000 000-1. |
| impulsów | int (4) | Szerokość obrazu w pikselach. |
| Wysokość | int (4) | Wysokość obrazu w pikselach. |

W przypadkach, gdy masz już bazę danych z innym schematem, można użyć właściwości **AlternateTextField**, **ImageUrlField**i **NavigateUrlField** do mapowania atrybutów AdRotator do istniejącej bazy danych. Aby wyświetlić dane z bazy danych w formancie AdRotator, Dodaj kontrolkę źródła danych do strony, skonfiguruj parametry połączenia dla kontrolki źródła danych, aby wskazywały na bazę danych, i ustaw właściwość **DataSourceID** formantu ADROTATOR na identyfikator kontroli źródła danych. W przypadkach, gdy istnieje potrzeba programistycznego konfigurowania programu AdRotator ads, użyj zdarzenia AdCreated. Zdarzenie AdCreated przyjmuje dwa parametry; jeden obiekt, a drugie wystąpienie elementu AdCreatedEventArgs. AdCreatedEventArgs jest odwołaniem do usługi AD, która jest tworzona.

Poniższy fragment kodu ustawia ImageUrl, NavigateUrl i AlternateText dla usługi AD programowo:

[!code-csharp[Main](data-bound-controls/samples/sample3.cs)]

## <a name="list-controls"></a>Kontrolki listy

Kontrolki list obejmują elementy ListBox, DropDownList, formant CheckBoxList, RadioButtonList i BulletedList. Każda z tych kontrolek może być danymi powiązanymi ze źródłem danych. Używają one jednego pola w źródle danych jako tekstu wyświetlanego i opcjonalnie mogą używać drugiego pola jako wartości elementu. Elementy można także dodać statycznie w czasie projektowania i można mieszać elementy statyczne i elementy dynamiczne dodane ze źródła danych.

Aby powiązać dane z kontrolką listy, Dodaj kontrolkę źródła danych do strony. Określ polecenie SELECT dla kontrolki źródła danych, a następnie ustaw właściwość DataSourceID kontrolki list na identyfikator kontrolki źródła danych. Użyj właściwości **DataTextField** i **DataValueField** , aby zdefiniować tekst wyświetlany i wartość kontrolki. Ponadto można użyć właściwości **DataTextFormatString** , aby kontrolować wygląd wyświetlanego tekstu w następujący sposób:

| **Wyrażenia** | **Opis** |
| --- | --- |
| Cena: {0:C} | Dla danych liczbowych/dziesiętnych. Wyświetla literał "Price:", po którym następuje cyfra w formacie waluty. Format waluty zależy od ustawienia kultury określonego w atrybucie Culture w dyrektywie **Page** lub w pliku Web. config. |
| {0:D4} | Dla danych liczb całkowitych. Nie można używać z liczbami dziesiętnymi. Liczby całkowite są wyświetlane w polu uzupełnionym od zera, które zawiera cztery znaki. |
| {0:N2}% | Dla danych liczbowych. Wyświetla liczbę z dokładnością do 2-dziesiętną, po której następuje literał "%". |
| {0:000.0} | Dla danych liczbowych/dziesiętnych. Liczby są zaokrąglane do jednego miejsca dziesiętnego. Liczby mniejsze niż trzy cyfry są uzupełnione o zero. |
| {0:D} | Dla danych daty/godziny. Wyświetla datę długa format ("czwartek, sierpień 06, 1996"). Format daty zależy od ustawienia kultury strony lub pliku Web. config. |
| {0:d} | Dla danych daty/godziny. Wyświetla format daty krótkiej ("12/31/99"). |
| {0:yy-MM-dd} | Dla danych daty/godziny. Wyświetla datę w formacie "rok-miesiąc-dzień" (96-08-06) |

## <a name="gridview"></a>GridView

Formant GridView umożliwia wyświetlanie i edytowanie danych tabelarycznych przy użyciu podejścia deklaracyjnego i jest następnikiem formantu DataGrid. W formancie GridView dostępne są następujące funkcje.

- Powiązanie z kontrolkami źródła danych, takimi jak kontrolki SqlDataSource.
- Wbudowane funkcje sortowania.
- Wbudowane funkcje aktualizowania i usuwania.
- Wbudowane funkcje stronicowania.
- Wbudowane funkcje wyboru wierszy.
- Programistyczny dostęp do modelu obiektów GridView w celu dynamicznego ustawiania właściwości, obsługi zdarzeń i tak dalej.
- Wiele pól kluczy.
- Wiele pól danych dla kolumn hiperłączy.
- Dostosowywalny wygląd poprzez motywy i style.

**Pola kolumn**

Każda kolumna w formancie GridView jest reprezentowana przez obiekt DataControlField. Domyślnie właściwość AutoGenerateColumns jest ustawiona na **wartość true**, co powoduje utworzenie obiektu AutoGeneratedField dla każdego pola w źródle danych. Każde pole jest następnie renderowane jako kolumna w kontrolce GridView w kolejności, w której każde pole pojawia się w źródle danych. Możesz również ręcznie kontrolować, które pola kolumny pojawiają się w kontrolce **GridView** , ustawiając właściwość **AutoGenerateColumns** na **false** , a następnie definiując własną kolekcję pól kolumn. Różne typy pól kolumn określają zachowanie kolumn w kontrolce.

W poniższej tabeli wymieniono różne typy pól kolumn, które mogą być używane.

| **Typ pola kolumny** | **Opis** |
| --- | --- |
| BoundField | Wyświetla wartość pola w źródle danych. Jest to domyślny typ kolumny kontrolki GridView. |
| ButtonField | Wyświetla przycisk polecenia dla każdego elementu w kontrolce GridView. Pozwala to na utworzenie kolumny niestandardowych kontrolek przycisków, takich jak przycisk Dodaj lub Usuń. |
| CheckBoxField | Wyświetla pole wyboru dla każdego elementu w kontrolce GridView. Ten typ pola kolumny jest często używany do wyświetlania pól o wartości logicznej. |
| CommandField | Wyświetla wstępnie zdefiniowane przyciski poleceń do wykonywania operacji zaznaczania, edytowania lub usuwania. |
| HyperLinkField | Wyświetla wartość pola w źródle danych jako hiperlink. Ten typ pola kolumny umożliwia powiązanie drugiego pola z adresem URL hiperłącza. |
| ImageField | Wyświetla obraz dla każdego elementu w kontrolce GridView. |
| TemplateField | Wyświetla zawartość zdefiniowaną przez użytkownika dla każdego elementu w formancie GridView zgodnie z określonym szablonem. Ten typ pola kolumny umożliwia utworzenie pola kolumny niestandardowej. |

Aby zdefiniować w sposób deklaratywny zbiór pól kolumn, najpierw Dodaj **&lt;kolumny** otwierające i zamykające&gt;tagów między tagiem otwierającym i zamykającym kontrolki GridView. Następnie należy wyświetlić listę pól kolumn, które mają być zawarte między&lt;otwierającymi i zamykanymi **kolumnami&gt;** . Określone kolumny są dodawane do kolekcji Columns w podanej kolejności. Kolekcja **Columns** przechowuje wszystkie pola kolumny w kontrolce i pozwala programowo zarządzać polami kolumn w kontrolce GridView.

Jawnie zadeklarowane pola kolumny mogą być wyświetlane w połączeniu z automatycznie wygenerowanymi polami kolumn. Gdy oba są używane, najpierw są renderowane jawnie zadeklarowane pola kolumny, a następnie pola automatycznie generowane kolumny.

## <a name="binding-to-data"></a>Wiązanie z danymi

Formant GridView można powiązać z kontrolką źródła danych (np. **kontrolki SqlDataSource**, **ObjectDataSource**itd.), a także ze wszystkimi źródłami danych, które implementują interfejs System. Collections. IEnumerable (na przykład system. Data. DataView, system. Collections. ArrayList lub system. Collections. Hashtable). Użyj jednej z następujących metod, aby powiązać formant GridView z odpowiednim typem źródła danych:

- Aby powiązać z kontrolką źródła danych, ustaw właściwość DataSourceID formantu GridView na wartość identyfikatora kontrolki źródła danych. Kontrolka GridView automatycznie wiąże się z określoną kontrolą źródła danych i może korzystać z możliwości kontroli źródła danych, aby wykonywać sortowanie, aktualizowanie, usuwanie i stronicowanie funkcji. Jest to preferowana metoda wiązania z danymi.
- Aby powiązać ze źródłem danych, które implementuje interfejs System. Collections. IEnumerable, programowo Ustaw Właściwość DataSource formantu GridView ze źródłem danych, a następnie Wywołaj metodę wiązania. W przypadku korzystania z tej metody formant GridView nie zapewnia wbudowanej funkcji sortowania, aktualizowania, usuwania i stronicowania. Musisz samodzielnie zapewnić tę funkcjonalność.

## <a name="data-operations"></a>Operacje na danych

Formant GridView zawiera wiele wbudowanych funkcji umożliwiających użytkownikowi sortowanie, aktualizowanie, usuwanie, wybieranie i wykonywanie stron za pomocą elementów w formancie. Gdy formant GridView jest powiązany z kontrolką źródła danych, formant GridView może korzystać z możliwości kontroli źródła danych i zapewniać automatyczne sortowanie, aktualizowanie i usuwanie funkcji.

> [!NOTE]
> Formant GridView może zapewnić obsługę sortowania, aktualizowania i usuwania innych typów źródeł danych. należy jednak zapewnić odpowiednią procedurę obsługi zdarzeń z implementacją tych operacji.

Sortowanie umożliwia użytkownikowi sortowanie elementów w formancie GridView w odniesieniu do określonej kolumny, klikając nagłówek kolumny. Aby włączyć sortowanie, ustaw właściwość AllowSorting na **wartość true**.

Funkcje automatycznego aktualizowania, usuwania i wyboru są włączane, gdy przycisk w polu kolumny **ButtonField** lub **TemplateField** jest kliknięty odpowiednio z nazwą polecenia "Edytuj", "Usuń" i "Select". Formant GridView może automatycznie dodać pole kolumny **CommandField** z przyciskiem Edytuj, Usuń lub zaznacz, jeśli właściwość AutoGenerateEditButton, AutoGenerateDeleteButton lub AutoGenerateSelectButton jest odpowiednio ustawiona na **wartość true**.

> [!NOTE]
> Wstawianie rekordów do źródła danych nie jest bezpośrednio obsługiwane przez kontrolkę GridView. Można jednak wstawiać rekordy przy użyciu kontrolki GridView w połączeniu z kontrolką DetailsView lub FormView.

Zamiast wyświetlania wszystkich rekordów w źródle danych w tym samym czasie, formant GridView może automatycznie zerwać rekordy do stron. Aby włączyć stronicowanie, ustaw właściwość Właściwość AllowPaging na **wartość true**.

## <a name="customizing-the-user-interface"></a>Personalizowanie interfejsu użytkownika

Wygląd kontrolki GridView można dostosować przez ustawienie właściwości stylu dla różnych części formantu. W poniższej tabeli wymieniono różne właściwości stylu.

| **Style — właściwość** | **Opis** |
| --- | --- |
| AlternatingRowStyle | Ustawienia stylu wierszy danych przemiennych w kontrolce GridView. Gdy ta właściwość jest ustawiona, wiersze danych są wyświetlane jako zmienne ustawień RowStyle i **AlternatingRowStyle** . |
| EditRowStyle | Ustawienia stylu dla wiersza edytowanego w kontrolce GridView. |
| EmptyDataRowStyle | Ustawienia stylu pustego wiersza danych wyświetlanego w kontrolce GridView, gdy źródło danych nie zawiera żadnych rekordów. |
| Stopka | Ustawienia stylu dla wiersza stopki kontrolki GridView. |
| HeaderStyle | Ustawienia stylu wiersza nagłówka kontrolki GridView. |
| PagerStyle | Ustawienia stylu wiersza modułu stronicowania kontrolki GridView. |
| RowStyle | Ustawienia stylu wierszy danych w kontrolce GridView. Gdy właściwość **AlternatingRowStyle** jest również ustawiona, wiersze danych są wyświetlane w postaci przemiennej między ustawieniami **RowStyle** i **AlternatingRowStyle** . |
| SelectedRowStyle | Ustawienia stylu dla zaznaczonego wiersza w kontrolce GridView. |

Można także pokazać lub ukryć różne części formantu. Poniższa tabela zawiera listę właściwości, które kontrolują, które części są wyświetlane lub ukryte.

| **Właściwość** | **Opis** |
| --- | --- |
| ShowFooter | Pokazuje lub ukrywa sekcję stopki kontrolki GridView. |
| ShowHeader | Pokazuje lub ukrywa sekcję nagłówka kontrolki GridView. |

### <a name="events"></a>Zdarzenia

Formant GridView zawiera kilka zdarzeń, dla których można programować. Pozwala to na uruchomienie niestandardowej procedury za każdym razem, gdy wystąpi zdarzenie. W poniższej tabeli wymieniono zdarzenia obsługiwane przez formant GridView.

| **Zdarzenie** | **Opis** |
| --- | --- |
| PageIndexChanged | Występuje po kliknięciu jednego z przycisków modułu stronicowania, ale gdy kontrolka GridView obsługuje operację stronicowania. To zdarzenie jest często używane, gdy trzeba wykonać zadanie po przejściu użytkownika do innej strony w formancie. |
| PageIndexChanging | Występuje po kliknięciu jednego z przycisków modułu stronicowania, ale zanim kontrolka GridView obsługuje operację stronicowania. To zdarzenie jest często używane do anulowania operacji stronicowania. |
| RowCancelingEdit | Występuje, gdy kliknięto przycisk anulowania wiersza, ale przed opuszczeniem trybu edycji przez formant GridView. To zdarzenie jest często używane do zatrzymania operacji anulowania. |
| RowCommand | Występuje po kliknięciu przycisku w kontrolce GridView. To zdarzenie jest często używane do wykonywania zadania po kliknięciu przycisku w kontrolce. |
| RowCreated | Występuje po utworzeniu nowego wiersza w kontrolce GridView. To zdarzenie jest często używane do modyfikowania zawartości wiersza podczas tworzenia wiersza. |
| RowDataBound | Występuje, gdy wiersz danych jest powiązany z danymi w kontrolce GridView. To zdarzenie jest często używane do modyfikowania zawartości wiersza, gdy wiersz jest powiązany z danymi. |
| RowDeleted | Występuje, gdy kliknięto przycisk usuwania wiersza, ale po usunięciu rekordu ze źródła danych przez formant GridView. To zdarzenie jest często używane do sprawdzania wyników operacji usuwania. |
| RowDeleting | Występuje, gdy kliknięto przycisk usuwania wiersza, ale przed usunięciem rekordu ze źródła danych przez formant GridView. To zdarzenie jest często używane do anulowania operacji usuwania. |
| RowEditing | Występuje po kliknięciu przycisku edycji wiersza, ale przed przejściem do trybu edycji przez formant GridView. To zdarzenie jest często używane do anulowania operacji edycji. |
| RowUpdated | Występuje po kliknięciu przycisku Aktualizuj wiersza, ale po zaktualizowaniu wiersza przez formant GridView. To zdarzenie jest często używane do sprawdzania wyników operacji aktualizacji. |
| RowUpdating | Występuje, gdy kliknięto przycisk aktualizacji wiersza, ale przed zaktualizowaniem wiersza przez formant GridView. To zdarzenie jest często używane do anulowania operacji aktualizowania. |
| SelectedIndexChanged | Występuje, gdy kliknięto przycisk wyboru wiersza, ale gdy kontrolka GridView obsługuje operację SELECT. To zdarzenie jest często używane do wykonywania zadania po wybraniu wiersza w kontrolce. |
| SelectedIndexChanging | Występuje, gdy kliknięto przycisk wyboru wiersza, ale zanim formant GridView obsługuje operację SELECT. To zdarzenie jest często używane do anulowania operacji wyboru. |
| Segregowa | Występuje, gdy zostanie kliknięte hiperłącze do sortowania kolumny, ale gdy kontrolka GridView obsługuje operację sortowania. To zdarzenie jest często używane do wykonywania zadania po kliknięciu hiperłącza przez użytkownika w celu posortowania kolumny. |
| Sortowanie | Występuje, gdy zostanie kliknięte hiperłącze do sortowania kolumny, ale zanim kontrolka GridView przechwytuje operację sortowania. To zdarzenie jest często używane do anulowania operacji sortowania lub wykonywania niestandardowej procedury sortowania. |

## <a name="formview"></a>FormView

Kontrolka FormView służy do wyświetlania pojedynczego rekordu ze źródła danych. Jest podobna do kontrolki DetailsView, z tą różnicą, że zamiast pól wierszy są wyświetlane szablony zdefiniowane przez użytkownika. Tworzenie własnych szablonów zapewnia większą elastyczność w zakresie kontrolowania sposobu wyświetlania danych. Kontrolka FormView obsługuje następujące funkcje:

- Powiązanie z kontrolkami źródła danych, takimi jak kontrolki SqlDataSource i ObjectDataSource.
- Wbudowane funkcje wstawiania.
- Wbudowane funkcje aktualizowania i usuwania.
- Wbudowane funkcje stronicowania.
- Programistyczny dostęp do modelu obiektów FormView do dynamicznego ustawiania właściwości, obsługi zdarzeń i tak dalej.
- Dostosowywalny wygląd za poorednictwem szablonów, motywów i stylów zdefiniowanych przez użytkownika.

## <a name="templates"></a>Szablony

Aby wyświetlić zawartość formantu FormView, należy utworzyć szablony dla różnych części formantu. Większość szablonów jest opcjonalna; należy jednak utworzyć szablon dla trybu, w którym formant jest skonfigurowany. Na przykład kontrolka FormView, która obsługuje wstawianie rekordów, musi mieć zdefiniowany szablon Wstaw element. W poniższej tabeli wymieniono różne szablony, które można utworzyć.

| **Typ szablonu** | **Opis** |
| --- | --- |
| EditItemTemplate | Definiuje zawartość dla wiersza danych, gdy kontrolka FormView jest w trybie edycji. Ten szablon zawiera zazwyczaj formanty wejściowe i przyciski poleceń, za pomocą których użytkownik może edytować istniejący rekord. |
| EmptyDataTemplate | Definiuje zawartość pustego wiersza danych wyświetlanego, gdy formant FormView jest powiązany ze źródłem danych, które nie zawiera żadnych rekordów. Ten szablon zazwyczaj zawiera zawartość, aby ostrzec użytkownika o tym, że źródło danych nie zawiera żadnych rekordów. |
| FooterTemplate | Definiuje zawartość dla wiersza stopki. Ten szablon zwykle zawiera dowolną dodatkową zawartość, która ma być wyświetlana w wierszu stopki. Alternatywnie można po prostu określić tekst do wyświetlenia w wierszu stopki przez ustawienie właściwości FooterText. |
| HeaderTemplate | Definiuje zawartość wiersza nagłówka. Ten szablon zwykle zawiera dowolną dodatkową zawartość, która ma być wyświetlana w wierszu nagłówka. Alternatywnie można po prostu określić tekst do wyświetlenia w wierszu nagłówka przez ustawienie właściwości HeaderText. |
| ItemTemplate | Definiuje zawartość dla wiersza danych, gdy kontrolka FormView jest w trybie tylko do odczytu. Ten szablon zwykle zawiera zawartość, aby wyświetlić wartości istniejącego rekordu. |
| InsertItemTemplate | Definiuje zawartość dla wiersza danych, gdy kontrolka FormView jest w trybie wstawiania. Ten szablon zawiera zazwyczaj formanty wejściowe i przyciski poleceń, za pomocą których użytkownik może dodać nowy rekord. |
| PagerTemplate | Definiuje zawartość wiersza stronicowania wyświetlanego po włączeniu funkcji stronicowania (gdy właściwość Właściwość AllowPaging ma **wartość true**). Ten szablon zawiera zazwyczaj kontrolki, w których użytkownik może przechodzić do innego rekordu. |

Kontrolki wejściowe w szablonie Edytuj element i szablon Wstaw element można powiązać z polami źródła danych przy użyciu wyrażenia dwukierunkowego powiązania. Dzięki temu formant FormView może automatycznie wyodrębniać wartości kontrolki wprowadzania dla operacji aktualizacji lub wstawiania. Wyrażenia powiązań dwukierunkowych zezwalają na kontrolki wejściowe w szablonie Edytuj element na automatyczne wyświetlanie oryginalnych wartości pól.

### <a name="binding-to-data"></a>Wiązanie z danymi

Formant FormView można powiązać z kontrolką źródła danych (np. **kontrolki SqlDataSource**, AccessDataSource, **ObjectDataSource** itd.) lub z dowolnym źródłem danych, który implementuje interfejs System. Collections. IEnumerable (takie jak system. Data. DataView, system. Collections. ArrayList i system. Collections. Hashtable). Użyj jednej z następujących metod, aby powiązać formant FormView z odpowiednim typem źródła danych:

- Aby powiązać z kontrolką źródła danych, ustaw właściwość DataSourceID kontrolki FormView na wartość identyfikatora kontrolki źródła danych. Formant FormView automatycznie wiąże się z określoną kontrolą źródła danych i może korzystać z możliwości kontroli źródła danych, aby wykonywać funkcje wstawiania, aktualizowania, usuwania i stronicowania. Jest to preferowana metoda wiązania z danymi.
- Aby powiązać ze źródłem danych, które implementuje interfejs **System. Collections. IEnumerable** , programowo Ustaw Właściwość DataSource formantu FormView ze źródłem danych, a następnie Wywołaj metodę wiązania. W przypadku korzystania z tej metody formant FormView nie zapewnia wbudowanej funkcji wstawiania, aktualizowania, usuwania i stronicowania. Należy podać tę funkcję przy użyciu odpowiedniego zdarzenia.

## <a name="data-operations"></a>Operacje na danych

Formant FormView zawiera wiele wbudowanych funkcji umożliwiających użytkownikowi aktualizowanie, usuwanie, wstawianie i wykonywanie stron za pomocą elementów w formancie. Gdy formant FormView jest powiązany z kontrolką źródła danych, formant FormView może korzystać z możliwości kontroli źródła danych i zapewniać automatyczne aktualizowanie, usuwanie, wstawianie i stronicowanie funkcji. Kontrolka FormView może zapewnić obsługę operacji Update, DELETE, INSERT i stronicowania w przypadku innych typów źródeł danych. Jednak należy zapewnić odpowiednią procedurę obsługi zdarzeń z implementacją tych operacji.

Ponieważ kontrolka FormView używa szablonów, nie zapewnia ona sposobu automatycznego generowania przycisków poleceń w celu wykonywania operacji aktualizowania, usuwania lub wstawiania. Należy ręcznie dołączyć te przyciski poleceń do odpowiedniego szablonu. Kontrolka FormView rozpoznaje niektóre przyciski, które mają właściwości **CommandName** ustawione na określone wartości. Poniższa tabela zawiera listę przycisków poleceń rozpoznawanych przez formant FormView.

| **Przycisk** | **Wartość polecenia** | **Opis** |
| --- | --- | --- |
| Cancel | "Cancel" | Służy do aktualizowania lub wstawiania operacji w celu anulowania operacji i odrzucania wartości wprowadzonych przez użytkownika. Kontrolka FormView następnie wraca do trybu określonego przez właściwość DefaultMode. |
| Usuń | Usunięty | Używane podczas usuwania operacji do usuwania wyświetlanego rekordu ze źródła danych. Wywołuje zdarzenia ItemDeleting i ItemDeleted. |
| Edytuj | Edytowania | Używane podczas aktualizowania operacji w celu umieszczenia kontrolki FormView w trybie edycji. Zawartość określona we właściwości **EditItemTemplate** jest wyświetlana dla wiersza danych. |
| Wstaw | Wstawienia | Używane podczas wstawiania operacji do próby wstawienia nowego rekordu w źródle danych przy użyciu wartości dostarczonych przez użytkownika. Wywołuje zdarzenia ItemInserting i ItemInserted. |
| Nowa | Nowy | Używane podczas wstawiania operacji do umieszczania kontrolki FormView w trybie wstawiania. Zawartość określona we właściwości **InsertItemTemplate** jest wyświetlana dla wiersza danych. |
| Stronic | Stronic | Używane w operacjach stronicowania do reprezentowania przycisku w wierszu modułu stronicowania, który wykonuje stronicowanie. Aby określić operację stronicowania, ustaw właściwość **CommandArgument** przycisku na "Next", "poprz", "First", "Last" lub indeks strony, do której chcesz przejść. Wywołuje zdarzenia PageIndexChanging i PageIndexChanged. |
| Aktualizacja | Aktualizacji | Używane podczas aktualizowania operacji w celu próby zaktualizowania wyświetlanego rekordu w źródle danych za pomocą wartości dostarczonych przez użytkownika. Wywołuje zdarzenia ItemUpdating i ItemUpdated. |

W przeciwieństwie do przycisku Usuń (co powoduje natychmiastowe usunięcie wyświetlanego rekordu), gdy kliknięto przycisk Edytuj lub nowy, formant FormView przechodzi odpowiednio do trybu edycji lub wstawiania. W trybie edycji zawartość znajdująca się we właściwości **EditItemTemplate** jest wyświetlana dla bieżącego elementu danych. Zwykle szablon Edytuj element jest zdefiniowany w taki sposób, że przycisk Edytuj jest zastępowany przyciskiem Aktualizuj i Anuluj. Kontrolki wejściowe, które są odpowiednie dla typu danych pola (takie jak pole tekstowe lub kontrolka CheckBox) są również zwykle wyświetlane z wartością pola, które użytkownik może zmodyfikować. Kliknięcie przycisku Aktualizuj aktualizuje rekord w źródle danych, a kliknięcie przycisku Anuluj powoduje opuszczenie wszelkich zmian.

Podobnie, zawartość znajdująca się we właściwości **InsertItemTemplate** jest wyświetlana dla elementu danych, gdy formant jest w trybie wstawiania. Szablon Wstaw element jest zwykle definiowany w taki sposób, aby nowy przycisk został zastąpiony przyciskiem Wstaw i Anuluj, a dla użytkownika są wyświetlane puste kontrolki wprowadzania wartości dla nowego rekordu. Kliknięcie przycisku Wstaw wstawia rekord w źródle danych, a kliknięcie przycisku Anuluj spowoduje opuszczenie wszelkich zmian.

Formant FormView zawiera funkcję stronicowania, która umożliwia użytkownikowi nawigację do innych rekordów w źródle danych. Po włączeniu wiersz modułu stronicowania jest wyświetlany w kontrolce FormView, która zawiera kontrolki Nawigacja strony. Aby włączyć stronicowanie, ustaw właściwość **Właściwość AllowPaging** na **wartość true**. Aby dostosować wiersz modułu stronicowania, można ustawić właściwości obiektów zawartych w PagerStyle i PagerSettings. Zamiast korzystać z wbudowanego interfejsu użytkownika wiersza modułu stronicowania, można utworzyć własny interfejs użytkownika przy użyciu właściwości **PagerTemplate** .

## <a name="customizing-the-user-interface"></a>Personalizowanie interfejsu użytkownika

Wygląd kontrolki FormView można dostosować przez ustawienie właściwości stylu dla różnych części formantu. W poniższej tabeli wymieniono różne właściwości stylu.

| **Style — właściwość** | **Opis** |
| --- | --- |
| EditRowStyle | Ustawienia stylu wiersza danych, gdy kontrolka FormView jest w trybie edycji. |
| EmptyDataRowStyle | Ustawienia stylu pustego wiersza danych wyświetlanego w kontrolce FormView, gdy źródło danych nie zawiera żadnych rekordów. |
| Stopka | Ustawienia stylu w wierszu stopki kontrolki FormView. |
| HeaderStyle | Ustawienia stylu wiersza nagłówka kontrolki FormView. |
| InsertRowStyle | Ustawienia stylu wiersza danych, gdy kontrolka FormView jest w trybie wstawiania. |
| PagerStyle | Ustawienia stylu dla wiersza modułu stronicowania wyświetlanego w kontrolce FormView, gdy funkcja stronicowania jest włączona. |
| RowStyle | Ustawienia stylu wiersza danych, gdy kontrolka FormView jest w trybie tylko do odczytu. |

## <a name="events"></a>Zdarzenia

Kontrolka FormView zawiera kilka zdarzeń, dla których można programować. Pozwala to na uruchomienie niestandardowej procedury za każdym razem, gdy wystąpi zdarzenie. W poniższej tabeli wymieniono zdarzenia obsługiwane przez formant FormView.

| **Zdarzenie** | **Opis** |
| --- | --- |
| ItemCommand | Występuje, gdy zostanie kliknięty przycisk w kontrolce FormView. To zdarzenie jest często używane do wykonywania zadania po kliknięciu przycisku w kontrolce. |
| ItemCreated | Występuje po utworzeniu wszystkich obiektów FormViewRow w kontrolce FormView. To zdarzenie jest często używane do modyfikowania wartości rekordu przed wyświetleniem. |
| ItemDeleted | Występuje, gdy kliknięto przycisk Usuń (przycisk z właściwością **CommandName** o wartości "Delete"), ale po usunięciu rekordu ze źródła danych przez formant FormView. To zdarzenie jest często używane do sprawdzania wyników operacji usuwania. |
| ItemDeleting | Występuje po kliknięciu przycisku Usuń, ale przed usunięciem przez formant FormView rekordu ze źródła danych. To zdarzenie jest często używane do anulowania operacji usuwania. |
| ItemInserted | Występuje, gdy kliknięto przycisk Wstaw (przycisk z właściwością **CommandName** o wartości "INSERT"), ale po wstawieniu tego rekordu przez formant FormView. To zdarzenie jest często używane do sprawdzania wyników operacji wstawiania. |
| ItemInserting | Występuje po kliknięciu przycisku wstawiania, ale przed wstawieniem rekordu przez formant FormView. To zdarzenie jest często używane do anulowania operacji wstawiania. |
| ItemUpdated | Występuje, gdy zostanie kliknięty przycisk aktualizacji (przycisk z właściwością **CommandName** o wartości "Update"), ale po zaktualizowaniu wiersza przez formant FormView. To zdarzenie jest często używane do sprawdzania wyników operacji aktualizacji. |
| ItemUpdating | Występuje po kliknięciu przycisku Aktualizuj, ale przed zaktualizowaniem rekordu przez formant FormView. To zdarzenie jest często używane do anulowania operacji aktualizacji. |
| ModeChanged | Występuje po zmianie trybów formantu FormView (do edycji, wstawienia lub trybu tylko do odczytu). To zdarzenie jest często używane do wykonywania zadania, gdy kontrolka FormView zmienia tryb. |
| ModeChanging | Występuje przed zmianą trybów formantu FormView (do edycji, wstawiania lub trybu tylko do odczytu). To zdarzenie jest często używane do anulowania zmiany trybu. |
| PageIndexChanged | Występuje po kliknięciu jednego z przycisków modułu stronicowania, ale gdy kontrolka FormView obsługuje operację stronicowania. To zdarzenie jest często używane, gdy trzeba wykonać zadanie, gdy użytkownik nawiguje do innego rekordu w kontrolce. |
| PageIndexChanging | Występuje po kliknięciu jednego z przycisków modułu stronicowania, ale przed przeprowadzeniem przez formant FormView operacji stronicowania. To zdarzenie jest często używane do anulowania operacji stronicowania. |

## <a name="detailsview"></a>DetailsView

Formant DetailsView służy do wyświetlania pojedynczego rekordu ze źródła danych w tabeli, gdzie każde pole rekordu jest wyświetlane w wierszu tabeli. Może być używany w połączeniu z kontrolką GridView dla scenariuszy ze szczegółami głównymi. Formant DetailsView obsługuje następujące funkcje:

- Powiązanie z kontrolkami źródła danych, takimi jak kontrolki SqlDataSource.
- Wbudowane funkcje wstawiania.
- Wbudowane funkcje aktualizowania i usuwania.
- Wbudowane funkcje stronicowania.
- Programistyczny dostęp do modelu obiektów DetailsView do dynamicznego ustawiania właściwości, obsługi zdarzeń i tak dalej.
- Dostosowywalny wygląd poprzez motywy i style.

## <a name="row-fields"></a>Pola wierszy

Każdy wiersz danych w formancie DetailsView jest tworzony przez zadeklarowanie kontrolki pola. Różne typy pól wierszy określają zachowanie wierszy w kontrolce. Formanty pól pochodzą od DataControlField. W poniższej tabeli wymieniono różne typy pól wierszy, które mogą być używane.

| **Typ pola kolumny** | **Opis** |
| --- | --- |
| BoundField | Wyświetla wartość pola w źródle danych jako tekst. |
| ButtonField | Wyświetla przycisk polecenia w kontrolce DetailsView. Pozwala to wyświetlić wiersz z kontrolką niestandardowego przycisku, np. przyciskiem Dodaj lub Usuń. |
| CheckBoxField | Wyświetla pole wyboru w kontrolce DetailsView. Ten typ pola wiersza jest często używany do wyświetlania pól o wartości logicznej. |
| CommandField | Wyświetla wbudowane przyciski poleceń do wykonywania operacji edycji, wstawiania lub usuwania w kontrolce DetailsView. |
| HyperLinkField | Wyświetla wartość pola w źródle danych jako hiperlink. Ten typ pola wiersza umożliwia powiązanie drugiego pola z adresem URL hiperłącza. |
| ImageField | Wyświetla obraz w kontrolce DetailsView. |
| TemplateField | Wyświetla zawartość zdefiniowaną przez użytkownika dla wiersza w kontrolce DetailsView, zgodnie z określonym szablonem. Ten typ pola wiersza umożliwia utworzenie niestandardowego pola wiersza. |

Domyślnie właściwość AutoGenerateRows jest ustawiona na **wartość true**, która powoduje automatyczne wygenerowanie powiązanego obiektu pola wiersza dla każdego pola typu możliwego do powiązania w źródle danych. Prawidłowe typy możliwe do powiązania to ciąg, DateTime, decimal, GUID i zestaw typów pierwotnych. Każde pole jest następnie wyświetlane w wierszu jako tekst, w kolejności, w której każde pole pojawia się w źródle danych.

Automatyczne generowanie wierszy zapewnia szybki i łatwy sposób wyświetlania wszystkich pól w rekordzie. Aby jednak korzystać z zaawansowanych możliwości formantu DetailsView, należy jawnie zadeklarować pola wiersza do uwzględnienia w kontrolce DetailsView. Aby zadeklarować pola wiersza, należy najpierw ustawić **wartość false**dla właściwości **AutoGenerateRows** . Następnie Dodaj otwarte i zamykające **&lt;pól&gt;** tagów między tagiem otwierającym i zamykającym kontrolki DetailsView. Na koniec należy wyświetlić listę pól wierszy, które mają zostać uwzględnione między polami otwierającymi i zamykającymi **&lt;&gt;** Tagi. Określone pola wiersza są dodawane do kolekcji Fields w podanej kolejności. Kolekcja **Fields** umożliwia programistyczne Zarządzanie polami wierszy w formancie DetailsView.

> [!NOTE]
> Automatycznie generowane pola wierszy nie są dodawane do kolekcji Fields.

## <a name="binding-to-data"></a>Wiązanie z danymi

Formant DetailsView można powiązać z kontrolką źródła danych, taką jak **kontrolki SqlDataSource** lub AccessDataSource lub z dowolnym źródłem danych, które implementuje interfejs System. Collections. IEnumerable, taki jak system. Data. DataView, system. Collections. ArrayList i system. Collections. Hashtable.

Użyj jednej z następujących metod, aby powiązać formant DetailsView z odpowiednim typem źródła danych:

- Aby powiązać z kontrolką źródła danych, ustaw właściwość DataSourceID formantu DetailsView na wartość identyfikatora kontrolki źródła danych. Kontrolka DetailsView automatycznie wiąże się z określoną kontrolką źródła danych. Jest to preferowana metoda wiązania z danymi.
- Aby powiązać ze źródłem danych, które implementuje interfejs **System. Collections. IEnumerable** , programowo Ustaw Właściwość DataSource formantu DetailsView ze źródłem danych, a następnie Wywołaj metodę wiązania.

## <a name="security"></a>Zabezpieczenia

Ta kontrolka może służyć do wyświetlania danych wejściowych użytkownika, co może obejmować złośliwy skrypt klienta. Sprawdź wszystkie informacje wysyłane z klienta pod kątem skryptu wykonywalnego, instrukcji SQL lub innego kodu przed wyświetleniem go w aplikacji. ASP.NET udostępnia funkcję walidacji żądania wejścia, aby blokować skrypt i kod HTML w danych wejściowych użytkownika.

## <a name="data-operations"></a>Operacje na danych

Kontrolka DetailsView udostępnia wbudowane funkcje umożliwiające użytkownikowi aktualizowanie, usuwanie, wstawianie i wykonywanie stron przez elementy w formancie. Gdy formant DetailsView jest powiązany z kontrolką źródła danych, formant DetailsView może korzystać z możliwości kontroli źródła danych i zapewniać automatyczne aktualizowanie, usuwanie, wstawianie i stronicowanie funkcji.

Formant DetailsView może zapewnić obsługę operacji Update, DELETE, INSERT i stronicowania w przypadku innych typów źródeł danych. należy jednak wprowadzić implementację tych operacji w odpowiedniej procedurze obsługi zdarzeń.

Formant DetailsView może automatycznie dodać pole wiersza **CommandField** z przyciskiem Edytuj, Usuń lub nowy, ustawiając odpowiednio właściwości AutoGenerateEditButton, AutoGenerateDeleteButton lub AutoGenerateInsertButton na **true**. W przeciwieństwie do przycisku Usuń (co powoduje natychmiastowe usunięcie wybranego rekordu), gdy kliknięto przycisk Edytuj lub nowy, formant DetailsView przechodzi odpowiednio do trybu edycji lub wstawiania. W trybie edycji przycisk Edytuj jest zastępowany przyciskiem Aktualizuj i Anuluj. Kontrolki wejściowe, które są odpowiednie dla typu danych pola (takie jak pole tekstowe lub kontrolka CheckBox) są wyświetlane z wartością pola, które użytkownik może zmodyfikować. Kliknięcie przycisku Aktualizuj aktualizuje rekord w źródle danych, a kliknięcie przycisku Anuluj powoduje opuszczenie wszelkich zmian. Podobnie w trybie wstawiania nowy przycisk jest zastępowany przyciskiem Wstaw i Anuluj, a dla użytkownika są wyświetlane puste kontrolki wprowadzania wartości dla nowego rekordu.

Formant DetailsView zawiera funkcję stronicowania, która umożliwia użytkownikowi nawigację do innych rekordów w źródle danych. Po włączeniu kontrolki nawigacji na stronach są wyświetlane w wierszu modułu stronicowania. Aby włączyć stronicowanie, ustaw właściwość Właściwość AllowPaging na **wartość true**. Wiersz modułu stronicowania można dostosować przy użyciu właściwości PagerStyle i PagerSettings.

## <a name="customizing-the-user-interface"></a>Personalizowanie interfejsu użytkownika

Wygląd formantu DetailsView można dostosować przez ustawienie właściwości stylu dla różnych części formantu. W poniższej tabeli wymieniono różne właściwości stylu.

| **Style — właściwość** | **Opis** |
| --- | --- |
| AlternatingRowStyle | Ustawienia stylu wierszy danych przemiennych w kontrolce DetailsView. Gdy ta właściwość jest ustawiona, wiersze danych są wyświetlane jako zmienne ustawień RowStyle i **AlternatingRowStyle** . |
| CommandRowStyle | Ustawienia stylu wiersza zawierającego wbudowane przyciski poleceń w kontrolce DetailsView. |
| EditRowStyle | Ustawienia stylu wierszy danych, gdy kontrolka DetailsView jest w trybie edycji. |
| EmptyDataRowStyle | Ustawienia stylu pustego wiersza danych wyświetlanego w kontrolce DetailsView, gdy źródło danych nie zawiera żadnych rekordów. |
| Stopka | Ustawienia stylu dla wiersza stopki kontrolki DetailsView. |
| HeaderStyle | Ustawienia stylu wiersza nagłówka kontrolki DetailsView. |
| InsertRowStyle | Ustawienia stylu wierszy danych, gdy kontrolka DetailsView jest w trybie wstawiania. |
| PagerStyle | Ustawienia stylu wiersza modułu stronicowania kontrolki DetailsView. |
| RowStyle | Ustawienia stylu wierszy danych w kontrolce DetailsView. Gdy właściwość **AlternatingRowStyle** jest również ustawiona, wiersze danych są wyświetlane w postaci przemiennej między ustawieniami **RowStyle** i **AlternatingRowStyle** . |
| FieldHeaderStyle | Ustawienia stylu kolumny nagłówka kontrolki DetailsView. |

## <a name="events"></a>Zdarzenia

Formant DetailsView zawiera kilka zdarzeń, dla których można programować. Pozwala to na uruchomienie niestandardowej procedury za każdym razem, gdy wystąpi zdarzenie. W poniższej tabeli wymieniono zdarzenia obsługiwane przez formant DetailsView. Kontrolka DetailsView dziedziczy również te zdarzenia z klas podstawowych: wiązania danych, powiązania danych, usuwania, init, load, PreRender i Render.

| **Zdarzenie** | **Opis** |
| --- | --- |
| ItemCommand | Występuje po kliknięciu przycisku w kontrolce DetailsView. |
| ItemCreated | Występuje po utworzeniu wszystkich obiektów DetailsViewRow w formancie DetailsView. To zdarzenie jest często używane do modyfikowania wartości rekordu przed wyświetleniem. |
| ItemDeleted | Występuje, gdy kliknięto przycisk Usuń, ale po usunięciu rekordu ze źródła danych przez formant DetailsView. To zdarzenie jest często używane do sprawdzania wyników operacji usuwania. |
| ItemDeleting | Występuje, gdy kliknięto przycisk Usuń, ale zanim formant DetailsView usunie rekord ze źródła danych. To zdarzenie jest często używane do anulowania operacji usuwania. |
| ItemInserted | Występuje po kliknięciu przycisku wstawiania, ale po wstawieniu tego rekordu przez formant DetailsView. To zdarzenie jest często używane do sprawdzania wyników operacji wstawiania. |
| ItemInserting | Występuje po kliknięciu przycisku wstawiania, ale przed wstawieniem rekordu przez formant DetailsView. To zdarzenie jest często używane do anulowania operacji wstawiania. |
| ItemUpdated | Występuje po kliknięciu przycisku Aktualizuj, ale po zaktualizowaniu wiersza przez formant DetailsView. To zdarzenie jest często używane do sprawdzania wyników operacji aktualizacji. |
| ItemUpdating | Występuje po kliknięciu przycisku aktualizacji, ale przed zaktualizowaniem rekordu przez formant DetailsView. To zdarzenie jest często używane do anulowania operacji aktualizacji. |
| ModeChanged | Występuje po zmianie trybów formantu DetailsView (Edytuj, Wstaw lub tryb tylko do odczytu). To zdarzenie jest często używane do wykonywania zadania, gdy kontrolka DetailsView zmienia tryb. |
| ModeChanging | Występuje przed zmianą trybów formantu DetailsView (Edycja, wstawianie lub tryb tylko do odczytu). To zdarzenie jest często używane do anulowania zmiany trybu. |
| PageIndexChanged | Występuje po kliknięciu jednego z przycisków modułu stronicowania, ale gdy kontrolka DetailsView obsługuje operację stronicowania. To zdarzenie jest często używane, gdy trzeba wykonać zadanie, gdy użytkownik nawiguje do innego rekordu w kontrolce. |
| PageIndexChanging | Występuje po kliknięciu jednego z przycisków modułu stronicowania, ale zanim kontrolka DetailsView obsługuje operację stronicowania. To zdarzenie jest często używane do anulowania operacji stronicowania. |

## <a name="the-menu-control"></a>Kontrolka menu

Formant menu w ASP.NET 2,0 został zaprojektowany jako w pełni funkcjonalny system nawigacji. Można łatwo powiązać dane z hierarchicznymi źródłami danych, takimi jak SiteMapDataSource.

Struktura kontrolek menu może być zdefiniowana deklaratywnie lub dynamicznie i składa się z jednego węzła głównego i dowolnej liczby węzłów podrzędnych. Poniższy kod deklaratywnie definiuje menu dla kontrolki menu.

[!code-aspx[Main](data-bound-controls/samples/sample4.aspx)]

W powyższym przykładzie węzeł Home. aspx jest węzłem głównym. Wszystkie inne węzły są zagnieżdżane w węźle głównym na różnych poziomach.

Istnieją dwa typy menu, które mogą być renderowane przez formant menu; menu statyczne i dynamiczne menu. Menu statyczne składają się z elementów menu, które są zawsze widoczne. Menu dynamiczne składają się z elementów menu, które są widoczne tylko wtedy, gdy użytkownik umieści nad nimi wskaźnikiem myszy. Klienci mogą często mylić statyczne menu z menu zdefiniowanymi deklaratywnie i dynamicznymi menu z menu, które są powiązane z danymi w czasie wykonywania. W rzeczywistości menu dynamiczne i statyczne nie są powiązane z metodą wypełniania. Terminy *statyczne* i *dynamiczne* odnoszą się tylko do tego, czy menu jest wyświetlane statycznie domyślnie, czy też wyświetlane tylko wtedy, gdy użytkownik wykona pewne działania.

Właściwość **StaticDisplayLevels** służy do konfigurowania, ile poziomów menu są statyczne i dlatego są domyślnie wyświetlane. W powyższym przykładzie ustawienie właściwości **StaticDisplayLevels** na wartość 2 spowodowałoby statyczne wyświetlanie węzła głównego, węzła muzyka i węzła filmów. Wszystkie inne węzły będą dynamicznie wyświetlane, gdy użytkownik umieści wskaźnik myszy nad węzłem nadrzędnym.

Właściwość **MaximumDynamicDisplayLevels** konfiguruje maksymalną liczbę poziomów dynamicznych, które mogą być wyświetlane w menu. Wszystkie dynamiczne menu na poziomie wyższym niż wartość określona przez właściwość **MaximumDynamicDisplayLevels** są odrzucane.

> [!NOTE]
> Niemal niektóre sytuacje, w których menu nie pojawiają się, nie są wyświetlane z powodu właściwości MaximumDynamicDisplayLevels. W takich przypadkach upewnij się, że właściwość została odpowiednio ustawiona, aby umożliwić wyświetlanie menu Customers.

## <a name="data-binding-the-menu-control"></a>Powiązanie danych kontrolki menu

Formant menu można powiązać z dowolnym hierarchicznym źródłem danych, takim jak SiteMapDataSource lub XmlDataSource. SiteMapDataSource jest najczęściej używaną metodą dla powiązania danych z kontrolką menu, ponieważ jest ona powiązana z plikiem Web. sitemap, a jego schemat udostępnia znany interfejs API dla kontrolki menu. Na poniższej liście przedstawiono prosty plik Web. sitemap.

[!code-xml[Main](data-bound-controls/samples/sample5.xml)]

Należy zauważyć, że w tym przypadku jest tylko jeden główny element siteMapNode, w tym przypadku element macierzysty. Dla każdego siteMapNode można skonfigurować kilka atrybutów. Najczęściej używane atrybuty są następujące:

- **adres URL** Określa adres URL, który będzie wyświetlany, gdy użytkownik kliknie element menu. Jeśli ten atrybut nie jest obecny, po kliknięciu węzeł zostanie po prostu Wyksięgowany ponownie.
- **tytuł** Określa tekst, który jest wyświetlany w elemencie menu.
- **Opis** Używane jako Dokumentacja dla węzła. Jest również wyświetlana jako etykietka narzędzia, gdy wskaźnik myszy znajduje się nad węzłem.
- **siteMapFile** Zezwala na zagnieżdżone mapy witryny. Ten atrybut musi wskazywać poprawnie sformułowany plik mapy ASP.NET.
- **role** Umożliwia sterowanie wyglądem węzła przez przycinanie ASP.NET zabezpieczeń.

Należy pamiętać, że chociaż te atrybuty są wszystkie opcjonalne, zachowanie menu może nie być oczekiwane, jeśli nie zostały określone. Na przykład jeśli atrybut *adresu URL* jest określony, ale atrybut *Description* nie jest, węzeł nie będzie widoczny i nie będzie można przejść do określonego adresu URL.

## <a name="controlling-a-menus-operation"></a>Kontrolowanie operacji menu

Istnieje kilka właściwości, które wpływają na działanie kontrolki menu ASP.NET; Właściwość **Orientation** , właściwość **DisappearAfter** , właściwość **StaticItemFormatString** i Właściwość **StaticPopoutImageUrl** są tylko niektórymi z nich.

- **Orientacja** może być ustawiona na *pozioma* lub *pionowa* i kontroluje, czy statyczne elementy menu są ułożone poziomo w wierszu czy w pionie i ułożone na siebie. Ta właściwość nie ma wpływu na menu dynamiczne.
- Właściwość **DisappearAfter** określa, jak długo menu dynamiczne ma pozostać widoczne po przesunięciu wskaźnika myszy od niego. Wartość jest określona w milisekundach i wartością domyślną jest 500. Ustawienie tej właściwości na wartość-1 spowoduje, że menu nigdy nie zniknie automatycznie. W takim przypadku menu będzie znikać tylko wtedy, gdy użytkownik kliknie poza menu.
- Właściwość **StaticItemFormatString** ułatwia zachowywanie spójnych otaczające w systemie menu. Podczas określania tej właściwości należy wprowadzić *{0}* zamiast opisu, który pojawia się w źródle danych. Na przykład, aby mieć element menu od ćwiczenia 1, odwiedź stronę naszych produktów itp., należy określić stronę z witryną {0} StaticItemFormatString. W czasie wykonywania ASP.NET zamieni każde wystąpienie {0} z prawidłowym opisem dla elementu menu.
- Właściwość **StaticPopoutImageUrl** określa obraz, który jest używany do wskazania, że konkretny węzeł menu ma węzły podrzędne, do których można uzyskać dostęp przez umieszczenie nad nim kursora. Menu dynamiczne będzie nadal używać domyślnego obrazu.

## <a name="templated-menu-controls"></a>Kontrolki menu z szablonami

Formant menu jest formantem z szablonem i umożliwia dla dwóch różnych elementów; StaticItemTemplate i DynamicItemTemplate. Za pomocą tych szablonów można łatwo dodawać kontrolki serwera lub kontrolki użytkownika do swoich menu.

Aby edytować szablony w programie Visual Studio .NET, kliknij przycisk tagów inteligentnych w menu, a następnie wybierz pozycję Edytuj szablony. Następnie można wybrać opcję edycji StaticItemTemplate lub DynamicItemTemplate.

Wszystkie kontrolki dodane do StaticItemTemplate pojawią się w menu statycznym podczas ładowania strony. Wszystkie kontrolki dodane do DynamicItemTemplate będą wyświetlane we wszystkich menu podręcznych.

## <a name="menu-events"></a>Zdarzenia menu

Kontrolka menu ma dwa zdarzenia, które są dla niego unikatowe. **MenuItemClicked** i **MenuItemDatabound** .

Zdarzenie MenuItemClicked jest wywoływane, gdy zostanie kliknięty element menu. Zdarzenie MenuItemDatabound jest wywoływane, gdy element menu jest powiązany z danymi. **MenuEventArgs** , który jest przesyłany do procedury obsługi zdarzeń, zapewnia dostęp do elementu menu za pośrednictwem właściwości Item.

## <a name="controlling-a-menus-appearance"></a>Kontrolowanie wyglądu menu

Możesz również mieć wpływ na wygląd kontrolki menu przy użyciu co najmniej jednego z wielu stylów dostępnych do formatowania menu. Należą do nich **StaticMenuStyle**, **DynamicMenuStyle**, **DynamicMenuItemStyle**, **DynamicSelectedStyle**i **DynamicHoverStyle**. Te właściwości są konfigurowane przy użyciu standardowego ciągu HTML. Na przykład poniższe zmiany wpłyną na styl menu dynamicznego.

[!code-aspx[Main](data-bound-controls/samples/sample6.aspx)]

> [!NOTE]
> Jeśli używasz dowolnego ze stylów aktywacji, musisz dodać element &lt;&gt; elementu na stronie do strony z elementem *runat* ustawionym na *serwer*.

Kontrolki menu obsługują również motywy ASP.NET 2,0.

## <a name="the-treeview-control"></a>Formant TreeView

Kontrolka TreeView wyświetla dane w strukturze podobnej do drzewa. Podobnie jak w przypadku kontrolki menu, można łatwo określić dane powiązane z dowolnym hierarchicznym źródłem danych, takim jak SiteMapDataSource.

Pierwsze pytanie, że klienci mogą zadać pytania dotyczące formantu TreeView w ASP.NET 2,0, to czy jest ono powiązane z formantem WebControl przeglądarki TreeView dostępnym dla ASP.NET 1. x. Nie jest. Kontrolka TreeView ASP.NET 2,0 została zapisywana od podstaw i oferuje znaczącą poprawę w porównaniu z kontrolką webformancie webtreeview, która była poprzednio dostępna.

Nie zajdziemy szczegółowo, jak powiązać formant TreeView z mapą witryny w miarę wykonywania w taki sam sposób jak w przypadku kontrolki menu. Jednak formant TreeView zawiera pewne odrębne różnice w sposobie, w jaki działa.

Domyślnie formant TreeView jest wyświetlany w pełni rozwinięte. Aby zmienić poziom rozwinięcia podczas początkowego ładowania, należy zmodyfikować właściwość **ExpandDepth** formantu. Jest to szczególnie ważne w przypadkach, gdy element TreeView jest powiązany z danymi podczas rozszerzania określonych węzłów.

## <a name="databinding-the-treeview-control"></a>Wiązanie danych kontrolki TreeView

W przeciwieństwie do kontrolki menu, obiekt TreeView zadecyduje również o obsłudze dużych ilości danych. W związku z tym oprócz wiązania z danymi typu SiteMapDataSource lub XmlDataSource, element TreeView jest często danymi powiązanymi z zestawem danych lub innymi danymi relacyjnymi. W przypadkach, gdy formant TreeView jest powiązany z dużymi ilościami danych, najlepiej powiązać tylko dane, które są faktycznie widoczne w formancie. Następnie można powiązać dane z dodatkowymi danymi, gdy węzły TreeView są rozwinięte.

W takich przypadkach Właściwość **PopulateOnDemand** elementu TreeView powinna mieć ustawioną *wartość true*. Następnie należy podać implementację metody **TreeNodePopulate** .

## <a name="data-binding-without-postback"></a>Powiązanie danych bez ogłaszania zwrotnego

Należy zauważyć, że po rozwinięciu węzła w poprzednim przykładzie po raz pierwszy strona zostanie odesłana i odświeżona. W tym przykładzie nie która problemu, ale można go wyobrazić w środowisku produkcyjnym z dużą ilością danych. Lepszym scenariuszem jest to, w którym element TreeView będzie nadal dynamicznie wypełniał węzły, ale bez ogłaszania zwrotnego na serwerze.

Ustawienie właściwości **PopulateNodesFromClient** i **PopulateOnDemand** na wartość true powoduje, że formant ASP.NET TreeView będzie dynamicznie wypełniać węzły bez publikowania zwrotnego. Po rozwinięciu węzła nadrzędnego żądanie XMLHTTP zostanie wykonane z klienta i zostanie wyzwolone zdarzenie OnTreeNodePopulate. Serwer odpowiada za pomocą Wyspy danych XML, który jest następnie używany do tworzenia powiązań danych z węzłami podrzędnymi.

ASP.NET dynamicznie tworzy kod klienta implementujący tę funkcję. Skrypt &lt;&gt; Tagi, które zawierają skrypt, są generowane wskazujących na plik AXD. Na przykład na poniższej liście przedstawiono linki do skryptu dla kodu skryptu, który generuje żądanie XMLHTTP.

[!code-html[Main](data-bound-controls/samples/sample7.html)]

Jeśli przeglądasz plik AXD powyżej w przeglądarce i otworzysz go, zobaczysz kod implementujący żądanie XMLHTTP. Ta metoda uniemożliwia klientom Modyfikowanie pliku skryptu.

## <a name="controlling-the-operation-of-the-treeview-control"></a>Kontrolowanie operacji formantu TreeView

Kontrolka TreeView ma kilka właściwości, które wpływają na działanie formantu. Najbardziej oczywiste właściwości to **ShowCheckBoxes**, **ShowExpandCollapse**i **ShowLines**.

Właściwość **ShowCheckBoxes** ma wpływ na to, czy w węzłach jest wyświetlane pole wyboru, które jest renderowane. Prawidłowe wartości dla tej właściwości to **none**, **root**, **Parent**, **Leaf**i **All**. Mają one wpływ na kontrolkę TreeView w następujący sposób:

| **Wartość właściwości** | **Efekt** |
| --- | --- |
| None | Pola wyboru nie są wyświetlane w żadnym węźle. Jest to ustawienie domyślne. |
| Pierwiastek | Pole wyboru jest wyświetlane tylko w węźle głównym. |
| Nadrzędny | Pole wyboru jest wyświetlane tylko w tych węzłach, które mają węzły podrzędne. Te węzły podrzędne mogą być węzłami nadrzędnymi lub węzłami liścia. |
| Liścia | Pole wyboru jest wyświetlane tylko w tych węzłach, które nie mają węzłów podrzędnych. |
| Wszystkie | Pole wyboru jest wyświetlane na wszystkich węzłach. |

Gdy pola wyboru są używane, właściwość **CheckedNodes** zwróci kolekcję węzłów TreeView, które są sprawdzane po odświeżeniu.

Właściwość **ShowExpandCollapse** steruje wyglądem obrazu Rozwiń/Zwiń obok węzłów głównych i nadrzędnych. Jeśli ta właściwość ma wartość **Fałsz**, węzły TreeView są renderowane jako hiperlinki i są rozwijane/zwijane przez kliknięcie linku.

Właściwość **ShowLines** określa, czy są wyświetlane linie łączące węzły nadrzędne z węzłami podrzędnymi. W przypadku **wartości false** (wartość domyślna) nie są wyświetlane żadne wiersze. W przypadku **wartości true**formant TreeView będzie używać obrazów linii w folderze określonym przez właściwość **LineImagesFolder** .

Aby dostosować wygląd wierszy TreeView, program Visual Studio .NET 2005 zawiera narzędzie Projektant linii. Dostęp do tego narzędzia można uzyskać za pomocą przycisku tagów inteligentnych w formancie TreeView, jak pokazano poniżej.

![](data-bound-controls/_static/image1.jpg)

**Rysunek 1**

Po wybraniu opcji menu **Dostosuj obrazy liniowe** zostanie uruchomione narzędzie Projektant wierszy, które umożliwi skonfigurowanie wyglądu wierszy w widoku TreeView.

## <a name="treeview-events"></a>Zdarzenia TreeView

Kontrolka TreeView ma następujące unikatowe zdarzenia:

- SelectedNodeChanged występuje po wybraniu węzła na podstawie właściwości **SelectAction** .
- TreeNodeCheckChanged występuje, gdy zmieni się stan pola wyboru węzłów.
- TreeNodeExpanded występuje, gdy węzeł jest rozwinięty na podstawie właściwości **SelectAction** .
- TreeNodeCollapsed występuje, gdy węzeł jest zwinięty.
- TreeNodeDataBound występuje, gdy węzeł jest powiązany z danymi.
- TreeNodePopulate występuje, gdy węzeł zostanie wypełniony.

Właściwość **SelectAction** umożliwia skonfigurowanie zdarzenia, które jest wyzwalane po wybraniu węzła. Właściwość SelectAction udostępnia następujące akcje:

- TreeNodeSelectAction. expand podnosi TreeNodeExpanded, gdy jest wybrany węzeł.
- TreeNodeSelectAction. None nie wywołuje żadnego zdarzenia, gdy węzeł jest wybrany.
- TreeNodeSelectAction. Select wywołuje zdarzenie SelectedNodeChanged, gdy jest wybrany węzeł.
- TreeNodeSelectAction. SelectExpand wywołuje zdarzenie SelectedNodeChanged i Zdarzenie TreeNodeExpanded, gdy jest wybrany węzeł.

## <a name="controlling-appearance-with-styles"></a>Kontrolowanie wyglądu przy użyciu stylów

Formant TreeView zawiera wiele właściwości do kontrolowania wyglądu kontrolki z stylami. Dostępne są następujące właściwości.

| **Nazwa właściwości** | **Kontrolki** |
| --- | --- |
| HoverNodeStyle | Kontroluje styl węzłów, gdy wskaźnik myszy znajduje się nad nimi. |
| LeafNodeStyle | Steruje stylem węzłów liścia. |
| Węzeł | Kontroluje styl dla wszystkich węzłów. Style określonego węzła (takie jak LeafNodeStyle) zastępują ten styl. |
| ParentNodeStyle | Kontroluje styl dla wszystkich węzłów nadrzędnych. |
| RootNodeStyle | Steruje stylem węzła głównego. |
| SelectedNodeStyle | Steruje stylem wybranego węzła. |

Każda z tych właściwości jest tylko do odczytu. Jednak będą one zwracały obiekt **TreeNode** , a właściwości tego obiektu można modyfikować za *pomocą formatu właściwości* . Na przykład, aby ustawić właściwość **ForeColor** **SelectedNodeStyle**, należy użyć następującej składni:

[!code-aspx[Main](data-bound-controls/samples/sample8.aspx)]

Zauważ, że powyższy tag nie jest zamknięty. Oznacza to, że w przypadku użycia składni deklaratywnej pokazanej w tym miejscu można również uwzględnić węzły TreeView w kodzie HTML.

Właściwości stylu można także określić w kodzie przy użyciu formatu *Property właściwości* . Na przykład, aby ustawić właściwość **ForeColor** **RootNodeStyle** w kodzie, należy użyć następującej składni:

[!code-csharp[Main](data-bound-controls/samples/sample9.cs)]

> [!NOTE]
> Aby zapoznać się z pełną listą różnych właściwości stylu, zobacz dokumentację MSDN w obiekcie TreeNodes.

## <a name="the-sitemappath-control"></a>Kontrolka ścieżki mapy witryny

Formant ścieżki mapy witryny udostępnia formant nawigacji Crumb dla deweloperów ASP.NET. Podobnie jak w przypadku innych kontrolek nawigacji, można je łatwo powiązać z hierarchicznymi źródłami danych, takimi jak SiteMapDataSource lub XmlDataSource.

Kontrolka ścieżki mapy witryny składa się z obiektów SiteMapNodeItem. Istnieją trzy typy węzłów: węzeł główny, węzły nadrzędne i bieżący węzeł. Węzeł główny jest węzłem w górnej części struktury hierarchicznej. Bieżący węzeł reprezentuje bieżącą stronę. Wszystkie inne węzły są węzłami nadrzędnymi.

## <a name="controlling-the-operation-of-the-sitemappath-control"></a>Kontrolowanie operacji formantu ścieżki mapy witryny

Właściwości kontrolujące operację formantu ścieżki mapy witryny są następujące:

| **Właściwość** | **Opis właściwości** |
| --- | --- |
| ParentLevelsDisplayed | Kontroluje liczbę węzłów nadrzędnych, które są wyświetlane. Wartość domyślna to-1, która nakłada brak ograniczeń dotyczących liczby wyświetlanych węzłów nadrzędnych. |
| PathDirection | Kontroluje kierunek ścieżki mapy witryny. Prawidłowe wartości to RootToCurrent (default) i CurrentToRoot. |
| PathSeparator | Ciąg, który kontroluje znak oddzielający węzły w kontrolce ścieżki mapy witryny. Wartość domyślna to:. |
| RenderCurrentNodeAsLink | Wartość logiczna, która kontroluje, czy bieżący węzeł jest renderowany jako link. Wartość domyślna to False. |
| SkipLinkText | Pomaga z ułatwieniami dostępu, gdy strona jest wyświetlana przez czytniki zawartości ekranu. Ta właściwość pozwala czytnikom ekranu pominąć formant ścieżki mapy witryny. Aby wyłączyć tę funkcję, należy ustawić właściwość String. Empty. |

## <a name="templated-sitemappath-controls"></a>Kontrolki ścieżki mapy witryny z szablonami

SiteMapControl jest formantem z szablonem, a w związku z tym można definiować różne szablony do użycia podczas wyświetlania formantu. Aby edytować szablony w kontrolce ścieżki mapy witryny, kliknij przycisk tagów inteligentnych na kontrolce i wybierz polecenie Edytuj szablony z menu. Spowoduje to wyświetlenie menu SiteMapTasks, jak pokazano poniżej, w którym można wybrać różne dostępne szablony.

![](data-bound-controls/_static/image2.jpg)

**Rysunek 2**

Szablon **NodeTemplate** odwołuje się do dowolnego węzła w ścieżki mapy witryny. Jeśli węzeł jest węzłem głównym lub jest skonfigurowany bieżący węzeł i **RootNodeTemplate** lub **CurrentNodeTemplate** , NodeTemplate zostanie zastąpiony.

## <a name="sitemappath-events"></a>Zdarzenia ścieżki mapy witryny

Kontrolka ścieżki mapy witryny ma dwa zdarzenia, które nie są wyprowadzane z klasy kontrolki; zdarzenia **ItemCreated** i **ItemDataBound** . Zdarzenie ItemCreated jest wywoływane po utworzeniu elementu ścieżki mapy witryny. ItemDataBound jest wywoływane, gdy wywoływana jest metoda DataBind podczas wiązania danych w węźle ścieżki mapy witryny. Obiekt **SiteMapNodeItemEventArgs** zapewnia dostęp do określonego SiteMapNodeItem za pośrednictwem właściwości Item.

## <a name="controlling-appearance-with-styles"></a>Kontrolowanie wyglądu przy użyciu stylów

Następujące style są dostępne do formatowania formantu ścieżki mapy witryny.

| **Nazwa właściwości** | **Kontrolki** |
| --- | --- |
| CurrentNodeStyle | Steruje stylem tekstu dla bieżącego węzła. |
| RootNodeStyle | Steruje stylem tekstu węzła głównego. |
| Węzeł | Steruje stylem tekstu dla wszystkich węzłów, przy założeniu, że element CurrentNodeStyle lub RootNodeStyle nie ma zastosowania. |

Właściwość nodename jest zastępowana przez CurrentNodeStyle lub RootNodeStyle. Każda z tych właściwości jest tylko do odczytu i zwraca obiekt **Style** . Aby mieć wpływ na wygląd węzła przy użyciu jednej z tych właściwości, należy ustawić właściwości obiektu stylu, który jest zwracany. Na przykład poniższy kod zmienia właściwość ForeColor bieżącego węzła.

[!code-aspx[Main](data-bound-controls/samples/sample10.aspx)]

Właściwość można również zastosować programowo w następujący sposób:

[!code-csharp[Main](data-bound-controls/samples/sample11.cs)]

> [!NOTE]
> Jeśli szablon zostanie zastosowany, styl nie zostanie zastosowany.

## <a name="lab-1-configuring-an-aspnet-menu-control"></a>Lab 1: Konfigurowanie kontrolki menu ASP.NET

1. Utwórz nową witrynę sieci Web.
2. Dodaj plik mapy witryny, wybierając kolejno pozycje plik i nowy plik, a następnie pozycję mapa witryny z listy szablonów plików.
3. Otwórz mapę witryny (domyślnie Web. sitemap) i zmodyfikuj ją tak, aby wyglądała jak na poniższej liście. Strony, do których tworzysz linki w pliku mapy witryny, nie istnieją, ale nie będą miały problemu dla tego ćwiczenia.

    [!code-xml[Main](data-bound-controls/samples/sample12.xml)]
4. Otwórz domyślny formularz sieci Web w widok Projekt.
5. Z sekcji nawigacyjnej przybornika Dodaj nową kontrolkę menu do strony.
6. Z sekcji dane w przyborniku Dodaj nowy SiteMapDataSource. SiteMapDataSource będzie automatycznie używać pliku Web. sitemap w Twojej lokacji. (Plik Web. sitemap *musi* znajdować się w folderze głównym witryny).
7. Kliknij formant menu, a następnie kliknij przycisk tagu inteligentnego, aby wyświetlić okno dialogowe zadania menu.
8. Z listy rozwijanej wybierz źródło danych wybierz pozycję SiteMapDataSource1.
9. Kliknij link Autoformatowanie i wybierz Format menu.
10. W okienku właściwości ustaw właściwość **StaticDisplayLevels** na wartość 2. Kontrolka menu powinna teraz wyświetlać węzeł Narzędzia główne, produkty i usługi w projektancie.
11. Przejrzyj stronę w przeglądarce, aby użyć menu. (Ponieważ strony dodane do mapy witryny nie istnieją w rzeczywistości, po próbie i przejściu do nich zostanie wyświetlony komunikat o błędzie).

Eksperymentuj z zmianą StaticDisplayLevels i właściwościami MaximumDynamicDisplayLevels i zobacz, jak mają wpływ na sposób renderowania menu.

## <a name="lab-2-dynamically-binding-a-treeview-control"></a>Lab 2: dynamiczne wiązanie kontrolki TreeView

W tym ćwiczeniu przyjęto założenie, że SQL Server uruchomione lokalnie i że baza danych Northwind znajduje się w wystąpieniu SQL Server. Jeśli te warunki nie są spełnione, należy zmienić parametry połączenia w przykładzie. Należy pamiętać, że może być również konieczne określenie uwierzytelniania SQL Server zamiast zaufanego połączenia.

1. Utwórz nową witrynę sieci Web.
2. Przełącz się do widoku kodu dla default. aspx i Zamień cały kod na kod wymieniony poniżej. 

    [!code-aspx[Main](data-bound-controls/samples/sample13.aspx)]
3. Zapisz stronę jako TreeView. aspx.
4. Przejrzyj stronę.
5. Gdy strona jest wyświetlana po raz pierwszy, Wyświetl źródło strony w przeglądarce. Należy zauważyć, że tylko widoczne węzły zostały wysłane do klienta.
6. Kliknij znak plus obok dowolnego węzła.
7. Ponownie Wyświetl źródło na stronie. Zauważ, że nowo wyświetlone węzły są teraz obecne.

## <a name="lab-3-details-view-and-editing-data-using-a-gridview-and-detailsview"></a>Lab 3: widok szczegółów i edytowanie danych przy użyciu widoku GridView i DetailsView

1. Utwórz nową witrynę sieci Web.
2. Dodaj nowy plik Web. config do witryny sieci Web.
3. Dodaj parametry połączenia do pliku Web. config, jak pokazano poniżej: 

    [!code-xml[Main](data-bound-controls/samples/sample14.xml)]

    > [!NOTE]
    > Może zajść konieczność zmiany parametrów połączenia w zależności od używanego środowiska.
4. Zapisz i zamknij plik Web. config.
5. Otwórz default. aspx i Dodaj nową kontrolkę kontrolki SqlDataSource.
6. Zmień identyfikator formantu kontrolki SqlDataSource na **produkty**.
7. W menu **zadania kontrolki SqlDataSource** kliknij pozycję **Konfiguruj źródło danych**.
8. Wybierz pozycję **Northwind** na liście rozwijanej połączenie i kliknij przycisk Dalej.
9. Wybierz pozycję **produkty** z listy rozwijanej **Nazwa** i zaznacz pola wyboru **ProductID**, **ProductName**, **CenaJednostkowa**i **UnitsInStock** , jak pokazano poniżej. 

![](data-bound-controls/_static/image3.jpg)

    **Figure 3**
10. Kliknij przycisk **Dalej**.
11. Kliknij przycisk **Finish** (Zakończ).
12. Przełącz się do widoku źródła i zbadaj wygenerowany kod. Zwróć uwagę na **Właściwość SelectCommand**, **DeleteCommand**, **InsertCommand**i **UpdateCommand** , która została dodana do kontrolki kontrolki SqlDataSource. Zwróć również uwagę na dodane parametry.
13. Przełącz się do widok Projekt i Dodaj do strony nowy formant GridView.
14. Wybierz pozycję **produkty** z listy rozwijanej **Wybierz źródło danych** .
15. Zaznacz pole wyboru **Włącz stronicowanie** i **Włącz wybór** , jak pokazano poniżej. 

![](data-bound-controls/_static/image4.jpg)

    **Figure 4**
16. Kliknij łącze **Edytuj kolumny** i upewnij się, że zaznaczone są **pola automatyczne generowanie** .
17. Kliknij przycisk **OK**.
18. Po wybraniu kontrolki GridView kliknij przycisk obok właściwości **DataKeyNames ustawionej** w okienku właściwości.
19. Wybierz pozycję **ProductID** z listy **dostępne pola danych** , a następnie kliknij przycisk **&gt;** , aby go dodać.
20. Kliknij przycisk OK.
21. Dodaj nową kontrolkę kontrolki SqlDataSource do strony.
22. Zmień identyfikator formantu kontrolki SqlDataSource na **szczegóły**.
23. W menu zadania kontrolki SqlDataSource wybierz polecenie **Konfiguruj źródło danych**.
24. Wybierz **Northwind** z listy rozwijanej i kliknij przycisk **dalej**.
25. Wybierz pozycję <strong>produkty</strong> z listy rozwijanej <strong>Nazwa</strong> i zaznacz pole wyboru <strong>\</strong > * w polu listy <strong>kolumn</strong> .
26. Kliknij przycisk **WHERE (gdzie** ).
27. Wybierz pozycję **ProductID** z listy rozwijanej **kolumny** .
28. Wybierz pozycję **=** na liście rozwijanej operator.
29. Wybierz pozycję **formant** z listy rozwijanej **Źródło** .
30. Wybierz pozycję **GridView1** z listy rozwijanej **Identyfikator kontrolki** .
31. Kliknij przycisk **Dodaj** , aby dodać klauzulę WHERE.
32. Kliknij przycisk **OK**.
33. Kliknij przycisk **Zaawansowane** i zaznacz pole wyboru **Generuj instrukcje INSERT, Update i DELETE** .
34. Kliknij przycisk **OK**.
35. Kliknij przycisk **dalej** , a następnie kliknij przycisk **Zakończ**.
36. Dodaj kontrolkę DetailsView do strony.
37. Z listy rozwijanej **Wybierz źródło danych** wybierz pozycję **szczegóły**.
38. Zaznacz pole wyboru **Włącz edytowanie** , jak pokazano poniżej. 

![](data-bound-controls/_static/image1.gif)

    **Figure 5**
39. Zapisz stronę i Przeglądaj default. aspx.
40. Kliknij link **Wybierz** obok innych rekordów, aby automatycznie zobaczyć aktualizację DetailsView.
41. Kliknij link **Edytuj** w kontrolce DetailsView.
42. Wprowadź zmiany w rekordzie i kliknij przycisk **Aktualizuj**.

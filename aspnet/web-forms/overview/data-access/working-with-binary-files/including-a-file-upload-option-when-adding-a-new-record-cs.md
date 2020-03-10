---
uid: web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-cs
title: Dołączanie opcji przekazywania pliku podczas dodawania nowego rekordu (C#) | Microsoft Docs
author: rick-anderson
description: W tym samouczku pokazano, jak utworzyć interfejs sieci Web, który umożliwia użytkownikowi wprowadzanie danych tekstowych i przekazywanie plików binarnych. Aby zilustrować dostępne opcje t...
ms.author: riande
ms.date: 03/27/2007
ms.assetid: 362ade25-3965-4fb2-88d2-835c4786244f
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-cs
msc.type: authoredcontent
ms.openlocfilehash: f1287e180151b3034a7b90ef4b3f1fbe68354a09
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78588285"
---
# <a name="including-a-file-upload-option-when-adding-a-new-record-c"></a>Dołączanie opcji przekazywania pliku podczas dodawania nowego rekordu (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_56_CS.exe) lub [Pobierz plik PDF](including-a-file-upload-option-when-adding-a-new-record-cs/_static/datatutorial56cs1.pdf)

> W tym samouczku pokazano, jak utworzyć interfejs sieci Web, który umożliwia użytkownikowi wprowadzanie danych tekstowych i przekazywanie plików binarnych. Aby zilustrować opcje dostępne do przechowywania danych binarnych, jeden plik zostanie zapisany w bazie danych, podczas gdy drugi zostanie zapisany w systemie plików.

## <a name="introduction"></a>Wprowadzenie

W poprzednich dwóch samouczkach zostały omówione metody przechowywania danych binarnych, które są skojarzone z modelem danych aplikacji, w jaki sposób używać kontrolki FileUpload do wysyłania plików z klienta do serwera sieci Web i dowiedzieć się, jak przedstawić te dane binarne w danych w formant EB. Jeszcze tu będziemy wiedzieć, jak skojarzyć przekazane dane z modelem danych.

W tym samouczku utworzymy stronę sieci Web, aby dodać nową kategorię. Oprócz pól tekstowych dla nazwy i opisu kategorii, ta strona będzie musiała zawierać dwie FileUpload kontrolki, jeden dla nowej ilustracji kategorii i jeden dla broszury. Przekazany obraz zostanie zapisany bezpośrednio w kolumnie nowy rekord `Picture`, podczas gdy Broszura zostanie zapisana w folderze `~/Brochures` z ścieżką do pliku zapisanego w kolumnie nowy rekord `BrochurePath`.

Przed utworzeniem nowej strony sieci Web należy zaktualizować architekturę. Zapytanie główne `CategoriesTableAdapter` s nie pobiera kolumny `Picture`. W związku z tym automatycznie generowana Metoda `Insert` ma tylko dane wejściowe dla pól `CategoryName`, `Description`i `BrochurePath`. W związku z tym musimy utworzyć dodatkową metodę w TableAdapter, która będzie monitował o wszystkie cztery `Categories` pól. Należy również zaktualizować klasę `CategoriesBLL` w warstwie logiki biznesowej.

## <a name="step-1-adding-aninsertwithpicturemethod-to-thecategoriestableadapter"></a>Krok 1: dodanie metody`InsertWithPicture`do`CategoriesTableAdapter`

Po utworzeniu `CategoriesTableAdapter` z powrotem w samouczku [Tworzenie warstwy dostępu do danych](../introduction/creating-a-data-access-layer-cs.md) został on skonfigurowany do automatycznego generowania instrukcji `INSERT`, `UPDATE`i `DELETE` na podstawie zapytania głównego. Ponadto firma Microsoft zaleca, aby TableAdapter wykorzystać podejście bezpośrednie DB, które spowodowało utworzenie metod `Insert`, `Update`i `Delete`. Te metody wykonują automatycznie generowane instrukcje `INSERT`, `UPDATE`i `DELETE` i w związku z tym akceptują parametry wejściowe na podstawie kolumn zwracanych przez zapytanie główne. W samouczku [przekazywania plików](uploading-files-cs.md) został rozszerzony zapytanie główne `CategoriesTableAdapter` s, aby użyć kolumny `BrochurePath`.

Ponieważ zapytanie główne `CategoriesTableAdapter` s nie odwołuje się do kolumny `Picture`, nie można dodać nowego rekordu ani zaktualizować istniejącego rekordu wartością dla kolumny `Picture`. Aby przechwycić te informacje, możemy utworzyć nową metodę w TableAdapter, która jest używana w celu wstawienia rekordu z danymi binarnymi, lub dostosować automatycznie wygenerowaną instrukcję `INSERT`. Problem z dostosowywaniem automatycznie generowanej instrukcji `INSERT` polega na tym, że istnieje ryzyko, że dostosowania zostały zastąpione przez kreatora. Załóżmy na przykład, że dostosowano instrukcję `INSERT`, aby uwzględnić użycie kolumny `Picture`. Spowoduje to zaktualizowanie metody TableAdapter `Insert` w celu uwzględnienia dodatkowego parametru wejściowego danych binarnych kategorii s. Następnie możemy utworzyć metodę w warstwie logiki biznesowej, aby użyć tej metody DAL i wywołać tę metodę LOGIKI biznesowej za pomocą warstwy prezentacji, a wszystko to bardzo wspaniałe. Oznacza to, że do czasu następnego skonfigurowania TableAdapter za pomocą Kreatora konfiguracji TableAdapter. Po zakończeniu pracy Kreatora nasze dostosowania instrukcji `INSERT` zostaną nadpisywane, `Insert` Metoda zostanie przywrócona do starego formularza i nasz kod nie będzie już kompilować!

> [!NOTE]
> Ten Annoyance nie jest problemem podczas korzystania z procedur składowanych zamiast instrukcji SQL ad hoc. W przyszłości zapoznaj się z użyciem procedur składowanych zamiast instrukcji SQL ad hoc w warstwie dostępu do danych.

Aby uniknąć tego potencjalnego kłopotliwej, zamiast dopasowywania automatycznie generowanych instrukcji SQL, zamiast tego należy utworzyć nową metodę dla TableAdapter. Ta metoda o nazwie `InsertWithPicture`akceptuje wartości kolumn `CategoryName`, `Description`, `BrochurePath`i `Picture`, a następnie wykonuje instrukcję `INSERT`, która przechowuje wszystkie cztery wartości w nowym rekordzie.

Otwórz wybrany zestaw danych i, w projektancie, kliknij prawym przyciskiem myszy nagłówek `CategoriesTableAdapter` s i wybierz polecenie Dodaj zapytanie z menu kontekstowego. Spowoduje to uruchomienie Kreatora konfiguracji zapytania TableAdapter, który rozpoczyna się od prośby do nas o to, jak zapytanie TableAdapter powinno uzyskać dostęp do bazy danych. Wybierz opcję Użyj instrukcji SQL i kliknij przycisk Dalej. W następnym kroku są wyświetlane monity o typ zapytania, które ma zostać wygenerowane. Ponieważ ponownie utworzysz zapytanie w celu dodania nowego rekordu do tabeli `Categories`, wybierz Wstaw i kliknij przycisk Dalej.

[![wybrać opcję wstawiania](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image1.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image1.png)

**Rysunek 1**. Wybierz opcję wstawiania ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image2.png))

Teraz należy określić `INSERT` instrukcji SQL. Kreator automatycznie sugeruje instrukcję `INSERT` odpowiadającą TableAdapter s głównej kwerendy. W tym przypadku jest to instrukcja `INSERT`, która wstawia `CategoryName`, `Description`i `BrochurePath` wartości. Zaktualizuj instrukcję, tak aby kolumna `Picture` była dołączona wraz z parametrem `@Picture`, na przykład:

[!code-sql[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample1.sql)]

Na ostatnim ekranie kreatora zostanie wyświetlony monit o podanie nazwy nowej metody TableAdapter. Wprowadź `InsertWithPicture` i kliknij przycisk Zakończ.

[![nazwę nowej metody TableAdapter InsertWithPicture](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image2.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image3.png)

**Rysunek 2**. Nazwa nowej metody TableAdapter `InsertWithPicture` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image4.png))

## <a name="step-2-updating-the-business-logic-layer"></a>Krok 2. aktualizowanie warstwy logiki biznesowej

Ponieważ warstwa prezentacji powinna tylko korzystać z warstwy logiki biznesowej, a nie pomijać jej do bezpośredniego przejścia do warstwy dostępu do danych, musimy utworzyć metodę LOGIKI biznesowej, która wywołuje metodę DAL, która została właśnie utworzona (`InsertWithPicture`). Na potrzeby tego samouczka Utwórz metodę w klasie `CategoriesBLL` o nazwie `InsertWithPicture`, która akceptuje jako dane wejściowe trzy `string` s i `byte` tablicę. Parametry wejściowe `string` są dla nazwy kategorii, opisu i ścieżki pliku broszury, natomiast tablica `byte` jest dla zawartości binarnej obrazu kategorii s. Jak przedstawiono w poniższym kodzie, Metoda LOGIKI biznesowej wywołuje odpowiednią metodę DAL:

[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample2.cs)]

> [!NOTE]
> Przed dodaniem metody `InsertWithPicture` do LOGIKI biznesowej upewnij się, że Zapisano określony zestaw danych. Ponieważ kod klasy `CategoriesTableAdapter` jest generowany automatycznie na podstawie określonego zestawu danych, jeśli nie zapiszesz zmian w określonym zestawie danych, właściwość `Adapter` nie będzie wiedzieć o metodzie `InsertWithPicture`.

## <a name="step-3-listing-the-existing-categories-and-their-binary-data"></a>Krok 3. Wyświetlanie istniejących kategorii i ich danych binarnych

W tym samouczku utworzymy stronę, która umożliwia użytkownikowi końcowemu dodanie nowej kategorii do systemu, dostarczając zdjęcie i broszurę dla nowej kategorii. W [poprzednim samouczku](displaying-binary-data-in-the-data-web-controls-cs.md) użyto widoku GridView z TemplateField i ImageField w celu wyświetlenia każdej kategorii, opisu, zdjęcia i linku, aby pobrać jego broszurę. Zezwól na replikację tej funkcji dla tego samouczka, tworząc stronę, która wyświetla listę wszystkich istniejących kategorii i umożliwia tworzenie nowych.

Zacznij od otwarcia strony `DisplayOrDownload.aspx` z folderu `BinaryData`. Przejdź do widoku źródła i skopiuj składnię "GridView" i "ObjectDataSource s", wklejając ją w elemencie `<asp:Content>` w `UploadInDetailsView.aspx`. Ponadto nie zapomnimy skopiować metody `GenerateBrochureLink` z klasy `DisplayOrDownload.aspx` do `UploadInDetailsView.aspx`.

[![skopiować i wkleić składnię deklaratywną z DisplayOrDownload. aspx do UploadInDetailsView. aspx](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image3.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image5.png)

**Rysunek 3**. Skopiuj i wklej składnię deklaratywną z `DisplayOrDownload.aspx` do `UploadInDetailsView.aspx` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image6.png))

Po skopiowaniu składni deklaratywnej i metodzie `GenerateBrochureLink` do strony `UploadInDetailsView.aspx` Wyświetl stronę za pośrednictwem przeglądarki, aby upewnić się, że wszystkie elementy zostały skopiowane pomyślnie. Powinien zostać wyświetlony widok GridView zawierający osiem kategorii, które zawierają link umożliwiający pobranie broszury oraz Zdjęcia kategorii.

[![powinna być teraz widoczna każda kategoria wraz z danymi binarnymi](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image4.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image7.png)

**Rysunek 4**. teraz powinna być widoczna każda kategoria wraz z danymi binarnymi ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image8.png))

## <a name="step-4-configuring-thecategoriesdatasourceto-support-inserting"></a>Krok 4. Konfigurowanie`CategoriesDataSource`do obsługi wstawiania

Element ObjectDataSource `CategoriesDataSource` używany przez `Categories` GridView obecnie nie zapewnia możliwości wstawiania danych. Aby można było obsługiwać Wstawianie przez tę kontrolkę źródła danych, musimy zmapować metodę `Insert` na metodę w obiekcie źródłowym, `CategoriesBLL`. W szczególności chcemy zmapować ją na metodę `CategoriesBLL`, która została dodana z powrotem w kroku 2, `InsertWithPicture`.

Zacznij od kliknięcia linku Konfiguruj źródło danych z tagu inteligentnego ObjectDataSource s. Pierwszy ekran pokazuje obiekt, z którym jest skonfigurowane źródło danych, `CategoriesBLL`. Pozostaw to ustawienie jako-is i kliknij przycisk Dalej, aby przejść do ekranu Definiowanie metod danych. Przejdź na kartę Wstawianie i wybierz metodę `InsertWithPicture` z listy rozwijanej. Kliknij przycisk Zakończ, aby zakończyć pracę kreatora.

[![skonfigurować element ObjectDataSource do korzystania z metody InsertWithPicture](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image5.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image9.png)

**Rysunek 5**. Konfigurowanie elementu ObjectDataSource do używania metody `InsertWithPicture` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image10.png))

> [!NOTE]
> Po zakończeniu działania kreatora program Visual Studio może zażądać odświeżenia pól i kluczy, co spowoduje ponowne wygenerowanie pól formantów sieci Web danych. Wybierz pozycję nie, ponieważ wybranie opcji Tak spowoduje zastąpienie wszelkich dostosowanych dostosowań pól.

Po zakończeniu działania kreatora element ObjectDataSource będzie teraz zawierał wartość właściwości `InsertMethod`, a także `InsertParameters` dla czterech kolumn kategorii, jak ilustruje to następujące deklaratywne znaczniki:

[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample3.aspx)]

## <a name="step-5-creating-the-inserting-interface"></a>Krok 5. Tworzenie interfejsu wstawiania

Jak pierwszy podano w [przeglądzie wstawiania, aktualizowania i usuwania danych](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md), formant DetailsView udostępnia wbudowany interfejs wstawiania, który może być używany podczas pracy z formantem źródła danych, który obsługuje wstawianie. Zezwól na dodanie kontrolki DetailsView do tej strony powyżej widoku GridView, która trwale renderuje swój interfejs wstawiania, umożliwiając użytkownikowi szybkie dodanie nowej kategorii. Po dodaniu nowej kategorii w widoku DetailsView zobaczysz automatycznie odświeżanie i wyświetlenie nowej kategorii.

Zacznij od przeciągnięcia widoku DetailsView z przybornika na projektanta nad GridView, ustawiając jego właściwość `ID` na `NewCategory` i czyszcząc wartości właściwości `Height` i `Width`. Z tagu inteligentnego DetailsView s powiąż go z istniejącym `CategoriesDataSource` a następnie zaznacz pole wyboru Włącz wstawianie.

[![powiązać widoku DetailsView z CategoriesDataSource i Włącz wstawianie](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image6.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image11.png)

**Ilustracja 6**. powiązanie widoku DetailsView z `CategoriesDataSource` i włączanie wstawiania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image12.png))

Aby trwale renderować DetailsView w interfejsie wstawiania, ustaw jej Właściwość `DefaultMode` na `Insert`.

Należy pamiętać, że element DetailsView ma pięć BoundFields `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`i `BrochurePath`, chociaż `CategoryID` BoundField nie jest renderowany w interfejsie wstawiania, ponieważ jego właściwość `InsertVisible` jest ustawiona na `false`. Te BoundFields istnieje, ponieważ są kolumnami zwracanymi przez metodę `GetCategories()`, która jest wywołaniem elementu ObjectDataSource w celu pobrania jego danych. Jednak w przypadku wstawiania nie chcemy pozwolić użytkownikowi na określenie wartości `NumberOfProducts`. Ponadto musimy zezwolić na przekazanie Zdjęcia dla nowej kategorii oraz przekazanie pliku PDF dla broszury.

Usuń `NumberOfProducts` BoundField z widoku DetailsView całkowicie, a następnie zaktualizuj odpowiednio właściwości `HeaderText` `CategoryName` i `BrochurePath` z kategorii i broszury. Następnie przekonwertuj `BrochurePath` BoundField na TemplateField i Dodaj nowy TemplateField dla obrazu, dając ten nowy TemplateField `HeaderText` wartość obrazu. Przenieś `Picture` TemplateField tak, aby był on między `BrochurePath` TemplateField i CommandField.

![Powiąż element DetailsView z CategoriesDataSource i Włącz wstawianie](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image7.gif)

**Rysunek 7**. powiązanie widoku DetailsView z `CategoriesDataSource` i włączanie wstawiania

W przypadku przekonwertowania `BrochurePath` BoundField do TemplateField za pomocą okna dialogowego Edycja pól TemplateField zawiera `ItemTemplate`, `EditItemTemplate`i `InsertItemTemplate`. Niemniej jednak tylko `InsertItemTemplate` jest to konieczne, aby usunąć pozostałe dwa szablony. W tym momencie składnia deklaratywna widoku DetailsView powinna wyglądać następująco:

[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample4.aspx)]

## <a name="adding-fileupload-controls-for-the-brochure-and-picture-fields"></a>Dodawanie kontrolek FileUpload do pól broszur i Picture

Obecnie `BrochurePath` TemplateField s `InsertItemTemplate` zawiera pole tekstowe, podczas gdy `Picture` TemplateField nie zawiera żadnych szablonów. Musimy zaktualizować te dwie TemplateField s `InsertItemTemplate` s, aby użyć formantów FileUpload.

Z tagu inteligentnego DetailsView s wybierz opcję Edytuj szablony, a następnie wybierz `BrochurePath` TemplateField s `InsertItemTemplate` z listy rozwijanej. Usuń pole tekstowe, a następnie przeciągnij kontrolkę FileUpload z przybornika do szablonu. Ustaw `ID` formantu FileUpload, aby `BrochureUpload`. Podobnie należy dodać formant FileUpload do `InsertItemTemplate``Picture` TemplateField. Ustaw ten FileUpload kontrolkę s `ID`, aby `PictureUpload`.

[![dodać kontrolkę FileUpload do InsertItemTemplate](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image8.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image13.png)

**Ilustracja 8**. Dodawanie kontrolki FileUpload do `InsertItemTemplate` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image14.png))

Po wprowadzeniu tych dodatków dwie TemplateFielda składnia deklaracyjne będzie:

[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample5.aspx)]

Gdy użytkownik doda nową kategorię, chcemy upewnić się, że Broszura i zdjęcie mają prawidłowy typ pliku. Dla broszury użytkownik musi podać plik PDF. W przypadku obrazu musimy przekazać użytkownikowi plik obrazu, ale zezwolimy na *każdy* plik obrazu lub tylko pliki obrazów określonego typu, takie jak GIF czy JPGs? Aby można było zezwolić na różne typy plików, musimy zwiększyć schemat `Categories`, aby uwzględnić kolumnę, która przechwytuje typ pliku, aby ten typ mógł zostać wysłany do klienta za pomocą `Response.ContentType` w `DisplayCategoryPicture.aspx`. Ze względu na to, że nie mamy takiej kolumny, warto zastanowić się, że użytkownicy będą mogli tylko dostarczać określony typ pliku obrazu. Istniejące obrazy `Categories` tabeli s są mapami bitowymi, ale JPGs są bardziej odpowiednim formatem pliku dla obrazów obsługiwanych przez sieć Web.

Jeśli użytkownik przekaże nieprawidłowy typ pliku, musi anulować Wstawianie i wyświetlić komunikat informujący o tym problemie. Dodaj kontrolkę sieci Web etykieta poniżej widoku DetailsView. Ustaw jej Właściwość `ID` na `UploadWarning`, wyczyść Właściwość `Text`, ustaw właściwość `CssClass` na Warning, a `Visible` i `EnableViewState` właściwości, aby `false`. Klasa `Warning` CSS jest definiowana w `Styles.css` i renderuje tekst w postaci długiej, czerwonej, kursywy, pogrubionej czcionki.

> [!NOTE]
> W idealnym przypadku `CategoryName` i `Description` BoundFields zostałyby przekonwertowane na używanie TemplateField i ich Wstawianie dostosowanych interfejsów. `Description` Wstawianie interfejsu, na przykład, prawdopodobnie będzie lepiej dopasowane przez wielowierszowe pole tekstowe. A ponieważ kolumna `CategoryName` nie akceptuje wartości `NULL`, należy dodać RequiredFieldValidator, aby upewnić się, że użytkownik poda wartość nowej nazwy kategorii. Te kroki są pozostawione jako ćwiczenia dla czytnika. Zapoznaj się z powrotem [, aby dostosować interfejs modyfikacji danych](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) , aby uzyskać szczegółowe informacje na temat rozszerzania interfejsów modyfikacji danych.

## <a name="step-6-saving-the-uploaded-brochure-to-the-web-server-s-file-system"></a>Krok 6. zapisywanie przekazanej broszury w systemie plików serwera sieci Web

Gdy użytkownik wprowadzi wartości dla nowej kategorii i kliknie przycisk Wstaw, następuje ogłaszanie zwrotne i wstawianie przepływu pracy. Najpierw zostanie wyzwolone zdarzenie DetailsView s [`ItemInserting`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserting.aspx) . Następnie zostanie wywołana metoda `Insert()` ObjectDataSource s, co spowoduje dodanie nowego rekordu do tabeli `Categories`. Następnie zostanie wyzwolone zdarzenie DetailsView s [`ItemInserted`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserted.aspx) .

Przed wywołaniem metody `Insert()` ObjectDataSource s należy najpierw upewnić się, że odpowiednie typy plików zostały przekazane przez użytkownika, a następnie zapisać plik PDF broszury w systemie plików serwera sieci Web. Utwórz procedurę obsługi zdarzeń dla zdarzenia `ItemInserting` DetailsView s i Dodaj następujący kod:

[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample6.cs)]

Program obsługi zdarzeń uruchamia się, odwołując się do kontrolki FileUpload `BrochureUpload` z szablonów DetailsView s. Następnie, jeśli broszura została przekazana, zostanie zbadane przekazane rozszerzenie pliku s. Jeśli rozszerzenie nie jest. Plik PDF, to ostrzeżenie jest wyświetlane, wstawianie zostało anulowane, a wykonywanie procedury obsługi zdarzeń zostanie zakończone.

> [!NOTE]
> Poleganie na przekazanym rozszerzeniu pliku s nie jest techniką "Uruchom jako", aby upewnić się, że przekazany plik jest dokumentem PDF. Użytkownik może mieć prawidłowy dokument PDF z rozszerzeniem `.Brochure`lub mógł zostać przeniesiony do dokumentu innego niż PDF i mieć rozszerzenie `.pdf`. Zawartość binarna pliku powinna być sprawdzana programowo w celu dokładniejszego zweryfikowania typu pliku. Takie dokładne podejście, chociaż często są zbyt obszerne; Sprawdzanie, czy rozszerzenie jest wystarczające dla większości scenariuszy.

Zgodnie z opisem w samouczku [przekazywanie plików](uploading-files-cs.md) należy zachować ostrożność podczas zapisywania plików w systemie plików, tak aby jedna z nich nie zastąpiła inna wartość. Na potrzeby tego samouczka Podejmiemy próbę użycia takiej samej nazwy jak przekazanego pliku. Jeśli istnieje już plik w katalogu `~/Brochures` o tej samej nazwie pliku, dołączymy numer na końcu do momentu znalezienia unikatowej nazwy. Na przykład, jeśli użytkownik przekazuje plik broszury o nazwie `Meats.pdf`, ale w folderze `~/Brochures` istnieje już plik o nazwie `Meats.pdf`, zmienimy nazwę zapisanego pliku na `Meats-1.pdf`. Jeśli tak się stanie, spróbujemy `Meats-2.pdf`i tak dalej, aż zostanie znaleziona unikatowa nazwa pliku.

Poniższy kod używa [metody`File.Exists(path)`](https://msdn.microsoft.com/library/system.io.file.exists.aspx) , aby określić, czy plik już istnieje z określoną nazwą pliku. W takim przypadku kontynuuje wypróbowanie nowych nazw plików dla broszury, dopóki nie zostanie znaleziony żaden konflikt.

[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample7.cs)]

Po znalezieniu prawidłowej nazwy pliku należy ją zapisać w systemie plików, a wartość elementu ObjectDataSource s `brochurePath``InsertParameter` należy zaktualizować, tak aby ta nazwa pliku była zapisywana w bazie danych. Po powrocie do samouczka *przekazywania plików* plik można zapisać przy użyciu metody FileUpload Control `SaveAs(path)`. Aby zaktualizować parametr `brochurePath` ObjectDataSource, Użyj kolekcji `e.Values`.

[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample8.cs)]

## <a name="step-7-saving-the-uploaded-picture-to-the-database"></a>Krok 7. zapisywanie przekazanego obrazu do bazy danych

Aby zapisać przekazany obraz w nowym rekordzie `Categories`, musimy przypisać przekazaną zawartość binarną do parametru `picture` ObjectDataSource s w zdarzeniu `ItemInserting` DetailsView. Przed przystąpieniem do tego przydziału musimy najpierw upewnić się, że przekazany obraz jest w formacie JPG, a nie innym typem obrazu. Jak w kroku 6, użyj do tego celu, aby upewnić się, że jest używany przekazany rozszerzenie pliku obrazu s.

Gdy tabela `Categories` umożliwia `NULL` wartości dla kolumny `Picture`, wszystkie kategorie mają obecnie obraz. Pozwól, aby użytkownicy mogli dostarczyć zdjęcie podczas dodawania nowej kategorii za pomocą tej strony. Poniższy kod sprawdza, czy zdjęcie zostało przekazane i ma odpowiednie rozszerzenie.

[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample9.cs)]

Ten kod powinien zostać umieszczony *przed* kodem z kroku 6, aby w przypadku wystąpienia problemu z przekazywaniem obrazu program obsługi zdarzeń zakończy działanie przed zapisaniem pliku broszury w systemie plików.

Przy założeniu, że został przekazany odpowiedni plik, przypisz przekazaną zawartość binarną do wartości parametru obrazu s następującym wierszem kodu:

[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample10.cs)]

## <a name="the-completeiteminsertingevent-handler"></a>Kompletna procedura obsługi zdarzeń`ItemInserting`

W pełni zamieszczono tutaj program obsługi zdarzeń `ItemInserting` w całości:

[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample11.cs)]

## <a name="step-8-fixing-thedisplaycategorypictureaspxpage"></a>Krok 8. naprawianie strony`DisplayCategoryPicture.aspx`

Poświęć chwilę na przetestowanie procedury obsługi zdarzeń wstawiania interfejsu i `ItemInserting`, która została utworzona w ciągu ostatnich kilku kroków. Odwiedź stronę `UploadInDetailsView.aspx` za pomocą przeglądarki, a następnie spróbuj dodać kategorię, ale pominąć obraz lub określić obraz inny niż JPG lub broszurę bez formatu PDF. W każdym z tych przypadków zostanie wyświetlony komunikat o błędzie i anulowano przepływ pracy wstawiania.

[![komunikat ostrzegawczy jest wyświetlany w przypadku przekazania nieprawidłowego typu pliku](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image9.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image15.png)

**Rysunek 9**. po przekazaniu nieprawidłowego typu pliku jest wyświetlany komunikat ostrzegawczy ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image16.png))

Po sprawdzeniu, czy strona wymaga przekazanie obrazu i nie akceptuje plików innych niż pliki PDF lub innych niż JPG, Dodaj nową kategorię z prawidłowym obrazem JPG, pozostawiając pole Broszura puste. Po kliknięciu przycisku Wstaw strona zostanie odpisana ponownie i zostanie dodany nowy rekord do tabeli `Categories` z przekazaną zawartością binarną obrazu s przechowywaną bezpośrednio w bazie danych. Widok GridView jest aktualizowany i zawiera wiersz dla nowo dodanej kategorii, ale, jak rysunek 10 pokazuje, nowy obraz kategorii nie jest poprawnie renderowany.

[![nowy obraz kategorii s nie jest wyświetlany](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image10.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image17.png)

**Ilustracja 10**. nowy obraz kategorii s nie jest wyświetlany ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image18.png))

Nie jest wyświetlany nowy obraz, ponieważ strona `DisplayCategoryPicture.aspx`, która zwraca określony obraz kategorii s, jest skonfigurowana do przetwarzania map bitowych, które mają nagłówek OLE. Ten nagłówek bajtowy 78 jest usuwany z zawartości binarnej `Picture` kolumn przed wysłaniem ich z powrotem do klienta. Ale plik JPG, który został właśnie przekazany dla nowej kategorii nie ma tego nagłówka OLE; w związku z tym prawidłowe, niezbędne bajty są usuwane z danych binarnych obrazu.

Ponieważ istnieją teraz obie mapy bitowe z nagłówkami OLE i JPGs w tabeli `Categories`, należy zaktualizować `DisplayCategoryPicture.aspx` tak, aby oddzielać nagłówek OLE dla oryginalnych ośmiu kategorii i pomijał to odróżnienie w przypadku nowszych rekordów kategorii. W naszym następnym samouczku sprawdzimy, jak zaktualizować istniejący obraz, i zaktualizujemy wszystkie stare obrazy kategorii, aby były JPGs. Na razie użyj poniższego kodu w `DisplayCategoryPicture.aspx`, aby rozdzielić nagłówki OLE tylko dla tych oryginalnych ośmiu kategorii:

[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample12.cs)]

Po tej zmianie obraz JPG jest teraz renderowany prawidłowo w widoku GridView.

[![obrazy JPG dla nowych kategorii są prawidłowo renderowane](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image11.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image19.png)

**Ilustracja 11**. obrazy jpg dla nowych kategorii są poprawnie renderowane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image20.png))

## <a name="step-9-deleting-the-brochure-in-the-face-of-an-exception"></a>Krok 9. Usuwanie broszury na początku wyjątku

Jednym z wyzwań związanych z przechowywaniem danych binarnych w systemie plików serwera sieci Web jest wprowadzenie rozłączenia między modelem danych i jego danymi binarnymi. W związku z tym, za każdym razem, gdy rekord zostanie usunięty, odpowiednie dane binarne w systemie plików również muszą zostać usunięte. To może być również odtwarzany podczas wstawiania. Rozważmy następujący scenariusz: użytkownik dodaje nową kategorię, określając prawidłowy obraz i broszurę. Po kliknięciu przycisku Wstaw następuje ogłaszanie zwrotne i wyzwalane jest zdarzenie `ItemInserting` DetailsView s, co umożliwia zapisanie broszury w systemie plików serwera sieci Web. Następnie wywoływana jest metoda `Insert()` ObjectDataSource s, która wywołuje metodę `InsertWithPicture` klasy `CategoriesBLL`, która wywołuje metodę `CategoriesTableAdapter` s `InsertWithPicture`.

Co się stanie, jeśli baza danych jest w trybie offline lub wystąpi błąd w instrukcji `INSERT` SQL? Wyraźnie Wstawianie zakończy się niepowodzeniem, dlatego do bazy danych nie zostanie dodany nowy wiersz kategorii. Jednak nadal mamy załadowanego pliku broszury w systemie plików serwera sieci Web. Ten plik musi zostać usunięty z powodu wyjątku podczas wstawiania przepływu pracy.

Jak opisano wcześniej w [obsłudze wyjątków logiki biznesowej i dal w samouczku dotyczącym strony ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) , gdy wyjątek jest zgłaszany z poziomu głębokości architektury, jest to możliwe za pośrednictwem różnych warstw. W warstwie prezentacji możemy określić, czy wystąpił wyjątek ze zdarzenia `ItemInserted` DetailsView s. Ten program obsługi zdarzeń udostępnia również wartości `InsertParameters`ObjectDataSource. W związku z tym możemy utworzyć procedurę obsługi zdarzeń dla zdarzenia `ItemInserted`, która sprawdza, czy wystąpił wyjątek i, jeśli tak, usuwa plik określony przez parametr `brochurePath` ObjectDataSource s:

[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample13.cs)]

## <a name="summary"></a>Podsumowanie

Istnieje kilka kroków, które należy wykonać, aby zapewnić interfejs oparty na sieci Web służący do dodawania rekordów zawierających dane binarne. Jeśli dane binarne są przechowywane bezpośrednio w bazie danych programu, prawdopodobnie trzeba będzie zaktualizować architekturę, dodając konkretne metody do obsługi przypadku, w którym dane binarne są wstawiane. Po zaktualizowaniu architektury następnym krokiem jest utworzenie interfejsu wstawiania, który można wykonać przy użyciu widoku DetailsView, który został dostosowany do uwzględnienia kontrolki FileUpload dla każdego pola danych binarnych. Przekazane dane można następnie zapisać w systemie plików serwera sieci Web lub przypisać do parametru źródła danych w programie obsługi zdarzeń w widoku DetailsView s `ItemInserting`.

Zapisywanie danych binarnych w systemie plików wymaga większego planowania niż zapisywanie danych bezpośrednio w bazie danych. Należy wybrać schemat nazewnictwa, aby uniknąć, że jeden użytkownik nie załaduje kolejnego elementu s. Ponadto należy podjąć dodatkowe kroki w celu usunięcia przekazanego pliku, jeśli wstawienie bazy danych nie powiedzie się.

Mamy teraz możliwość dodawania nowych kategorii do systemu z broszurą i obrazem, ale jeszcze tu zawarto informacje na temat aktualizowania istniejących danych binarnych kategorii lub sposobu poprawnego usuwania danych binarnych dla usuniętej kategorii. Te dwa tematy zostaną omówione w następnym samouczku.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci liderzy dla tego samouczka to Dave Gardner, Teresa Murphy i Bernadette Leigh. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](displaying-binary-data-in-the-data-web-controls-cs.md)
> [dalej](updating-and-deleting-existing-binary-data-cs.md)

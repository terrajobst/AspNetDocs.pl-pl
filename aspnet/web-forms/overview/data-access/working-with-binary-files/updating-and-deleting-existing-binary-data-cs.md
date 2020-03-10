---
uid: web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-cs
title: Aktualizowanie i usuwanie istniejących danych binarnychC#() | Microsoft Docs
author: rick-anderson
description: We wcześniejszych samouczkach przedstawiono sposób, w jaki formant GridView ułatwia edytowanie i usuwanie danych tekstowych. W tym samouczku zobaczymy, jak również formant GridView...
ms.author: riande
ms.date: 03/27/2007
ms.assetid: 35798f21-1606-434b-83f8-30166906ef49
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 3e37381ee48fcda8e0e10374aa7a6ae53c3cc77c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78587823"
---
# <a name="updating-and-deleting-existing-binary-data-c"></a>Aktualizowanie i usuwanie istniejących danych binarnych (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_57_CS.exe) lub [Pobierz plik PDF](updating-and-deleting-existing-binary-data-cs/_static/datatutorial57cs1.pdf)

> We wcześniejszych samouczkach przedstawiono sposób, w jaki formant GridView ułatwia edytowanie i usuwanie danych tekstowych. W tym samouczku widać, jak formant GridView umożliwia także edytowanie i usuwanie danych binarnych, niezależnie od tego, czy dane binarne są zapisywane w bazie danych, czy przechowywane w systemie plików.

## <a name="introduction"></a>Wprowadzenie

W ciągu ostatnich trzech samouczków dodaliśmy całkiem nieznacznie funkcję do pracy z danymi binarnymi. Rozpoczęto od dodania kolumny `BrochurePath` do tabeli `Categories` i odpowiednio Zaktualizowano architekturę. Dodano również metody warstwy dostępu do danych i warstwy logiki biznesowej do pracy z tabelami kategorii istniejące `Picture` kolumnie, która przechowuje zawartość binarną w pliku obrazu. Mamy wbudowaną stronę sieci Web, aby przedstawić dane binarne w widoku GridView linku do pobierania dla broszury, z kategorią s wyświetlaną w `<img>` elemencie i dodałem widok DetailsView, aby umożliwić użytkownikom dodawanie nowej kategorii i przekazywanie jej broszur i danych zdjęć.

Wszystkie te, które pozostało do zaimplementowania, to możliwość edytowania i usuwania istniejących kategorii, które wykonamy w tym samouczku przy użyciu wbudowanego edytowania i usuwania funkcji GridView. Podczas edytowania kategorii użytkownik będzie mógł opcjonalnie przekazać nowy obraz lub mieć kategorię nadal używa istniejącej. W przypadku broszury mogą wybrać opcję użycia istniejącej broszury, przekazać nową broszurę lub wskazać, że Kategoria nie ma już skojarzonej z nią broszury. Zacznij korzystać z aplikacji.

## <a name="step-1-updating-the-data-access-layer"></a>Krok 1. aktualizowanie warstwy dostępu do danych

DAL ma automatycznie generowane metody `Insert`, `Update`i `Delete`, ale te metody zostały wygenerowane na podstawie zapytania głównego `CategoriesTableAdapter` s, które nie zawiera kolumny `Picture`. W związku z tym metody `Insert` i `Update` nie obejmują parametrów służących do określania danych binarnych dla obrazu kategorii s. Podobnie jak w [poprzednim samouczku](including-a-file-upload-option-when-adding-a-new-record-cs.md), musimy utworzyć nową metodę TableAdapter do aktualizowania tabeli `Categories` podczas określania danych binarnych.

Otwórz wybrany zestaw danych i, w projektancie, kliknij prawym przyciskiem myszy nagłówek `CategoriesTableAdapter` s i wybierz polecenie Dodaj zapytanie z menu kontekstowego, aby uruchomić Kreatora konfiguracji zapytania TableAdapter. Ten Kreator zostanie uruchomiony z prośbą o to, w jaki sposób zapytanie TableAdapter powinno uzyskać dostęp do bazy danych. Wybierz opcję Użyj instrukcji SQL i kliknij przycisk Dalej. W następnym kroku są wyświetlane monity o typ zapytania, które ma zostać wygenerowane. Ponieważ ponownie utworzysz zapytanie w celu dodania nowego rekordu do tabeli `Categories`, wybierz polecenie Aktualizuj i kliknij przycisk Dalej.

[![wybrać opcję aktualizacji](updating-and-deleting-existing-binary-data-cs/_static/image1.gif)](updating-and-deleting-existing-binary-data-cs/_static/image1.png)

**Rysunek 1**. Wybierz opcję aktualizacji ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](updating-and-deleting-existing-binary-data-cs/_static/image2.png))

Teraz należy określić `UPDATE` instrukcji SQL. Kreator automatycznie sugeruje instrukcję `UPDATE` odpowiadającą TableAdapter s głównej kwerendy (jeden, która aktualizuje `CategoryName`, `Description`i `BrochurePath` wartości). Zmień instrukcję tak, aby kolumna `Picture` była dołączona wraz z parametrem `@Picture`, na przykład:

[!code-sql[Main](updating-and-deleting-existing-binary-data-cs/samples/sample1.sql)]

Na ostatnim ekranie kreatora zostanie wyświetlony monit o podanie nazwy nowej metody TableAdapter. Wprowadź `UpdateWithPicture` i kliknij przycisk Zakończ.

[![nazwę nowej metody TableAdapter UpdateWithPicture](updating-and-deleting-existing-binary-data-cs/_static/image2.gif)](updating-and-deleting-existing-binary-data-cs/_static/image3.png)

**Rysunek 2**. Nazwa nowej metody TableAdapter `UpdateWithPicture` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-and-deleting-existing-binary-data-cs/_static/image4.png))

## <a name="step-2-adding-the-business-logic-layer-methods"></a>Krok 2. Dodawanie metod warstwy logiki biznesowej

Oprócz aktualizowania DAL musimy zaktualizować LOGIKI biznesowej w celu uwzględnienia metod aktualizacji i usuwania kategorii. Są to metody, które będą wywoływane z warstwy prezentacji.

Aby można było usunąć kategorię, możemy użyć automatycznie generowanej metody `Delete` `CategoriesTableAdapter` s. Dodaj następującą metodę do klasy `CategoriesBLL`:

[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample2.cs)]

W tym samouczku pomogąmy tworzyć dwie metody aktualizowania kategorii — jedną, która oczekuje danych obrazu binarnego i wywołuje metodę `UpdateWithPicture`, która została dodana do `CategoriesTableAdapter`, a druga, która akceptuje tylko wartości `CategoryName`, `Description`i `BrochurePath`, a następnie używa `CategoriesTableAdapter`ej automatycznie generowanej instrukcji `Update` klasy s. Racjonalne uzasadnienie przy użyciu dwóch metod polega na tym, że użytkownik może chcieć zaktualizować obraz kategorii wraz z innymi polami, w takim przypadku użytkownik będzie musiał przekazać nowy obraz. Przekazane dane binarne obrazu można następnie użyć w instrukcji `UPDATE`. W innych przypadkach użytkownik może chcieć tylko zaktualizować, powiedzieć, nazwę i opis. Ale jeśli instrukcja `UPDATE` oczekuje również danych binarnych dla kolumny `Picture`, należy podać również te informacje. Będzie to wymagało dodatkowej podróży do bazy danych w celu przywrócenia danych obrazu dla edytowanego rekordu. W związku z tym chcemy mieć dwie `UPDATE` metody. Warstwa logiki biznesowej określi, która z nich ma być używana w zależności od tego, czy dane obrazu są dostarczane podczas aktualizowania kategorii.

Aby to ułatwić, należy dodać dwie metody do klasy `CategoriesBLL`, zarówno o nazwie `UpdateCategory`. Pierwszy z nich powinien przyjmować trzy `string` s, tablicę `byte` i `int` jako parametry wejściowe; sekunda, zaledwie trzy `string` s i `int`. Parametry wejściowe `string` są dla nazwy kategorii, opisu i ścieżki pliku broszury, tablica `byte` jest dla zawartości binarnej obrazu kategorii s, a `int` identyfikuje `CategoryID` rekordu do zaktualizowania. Należy zauważyć, że pierwsze Przeciążenie wywołuje sekundę, jeśli `byte` tablicy, która została przeniesiona, jest `null`:

[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample3.cs)]

## <a name="step-3-copying-over-the-insert-and-view-functionality"></a>Krok 3. Kopiowanie w ramach funkcji wstawiania i wyświetlania

W [poprzednim samouczku](including-a-file-upload-option-when-adding-a-new-record-cs.md) utworzyliśmy stronę o nazwie `UploadInDetailsView.aspx`, która jest wyświetlana na liście wszystkie kategorie w widoku GridView i udostępniono widok DetailsView w celu dodania nowych kategorii do systemu. W tym samouczku połączymy widok GridView w celu uwzględnienia funkcji edycji i usuwania. Zamiast kontynuować pracę z `UploadInDetailsView.aspx`, Zezwól na umieszczenie tych samouczków na stronie `UpdatingAndDeleting.aspx` z tego samego folderu, `~/BinaryData`. Skopiuj i wklej deklaracyjne znaczniki i kod z `UploadInDetailsView.aspx`, aby `UpdatingAndDeleting.aspx`.

Zacznij od otwarcia strony `UploadInDetailsView.aspx`. Skopiuj całą składnię deklaratywną w ramach elementu `<asp:Content>`, jak pokazano na rysunku 3. Następnie otwórz `UpdatingAndDeleting.aspx` i wklej ten znacznik w obrębie elementu `<asp:Content>`. Analogicznie Skopiuj kod z klasy `UploadInDetailsView.aspx` s z kodem, aby `UpdatingAndDeleting.aspx`.

[![skopiować deklaratywne znaczniki z UploadInDetailsView. aspx](updating-and-deleting-existing-binary-data-cs/_static/image3.gif)](updating-and-deleting-existing-binary-data-cs/_static/image5.png)

**Rysunek 3**. Kopiuj deklaratywne znaczniki z `UploadInDetailsView.aspx` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](updating-and-deleting-existing-binary-data-cs/_static/image6.png))

Po skopiowaniu do deklaratywnego znaczników i kodu, odwiedź `UpdatingAndDeleting.aspx`. Powinny być widoczne te same dane wyjściowe i mieć to samo środowisko użytkownika, co w przypadku `UploadInDetailsView.aspx` stronie z poprzedniego samouczka.

## <a name="step-4-adding-deleting-support-to-the-objectdatasource-and-gridview"></a>Krok 4. Dodawanie obsługi usuwania do elementu ObjectDataSource i GridView

Zgodnie z opisem w temacie [Omówienie wstawiania, aktualizowania i usuwania danych](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) , w widoku GridView widoczne są wbudowane funkcje usuwania, a funkcje te można włączyć w znaczniku wyboru, jeśli źródło danych źródłowej siatki obsługuje usuwanie. Obecnie element ObjectDataSource, z którym jest powiązany element GridView (`CategoriesDataSource`) nie obsługuje usuwania.

Aby to naprawić, kliknij opcję Konfiguruj źródło danych w tagu inteligentnym ObjectDataSource s, aby uruchomić kreatora. Pierwszy ekran pokazuje, że element ObjectDataSource jest skonfigurowany do pracy z klasą `CategoriesBLL`. Naciśnij przycisk Dalej. Obecnie są określone tylko właściwości `InsertMethod` i `SelectMethod` elementu ObjectDataSource. Jednak Kreator automatycznie wypełnia listę rozwijaną na kartach Aktualizuj i Usuń odpowiednio do `UpdateCategory` i `DeleteCategory`. Jest to spowodowane tym, że w klasie `CategoriesBLL`, która oznaczył te metody przy użyciu `DataObjectMethodAttribute` jako metody domyślne do aktualizowania i usuwania.

Na razie Ustaw listę rozwijaną Aktualizuj kartę na (brak), ale pozostaw listę rozwijaną Usuń kartę z ustawioną na `DeleteCategory`. Powrócimy do tego kreatora w kroku 6, aby dodać obsługę aktualizacji.

[![skonfigurować element ObjectDataSource do korzystania z metody DeleteCategory](updating-and-deleting-existing-binary-data-cs/_static/image4.gif)](updating-and-deleting-existing-binary-data-cs/_static/image7.png)

**Ilustracja 4**. Konfigurowanie elementu ObjectDataSource do używania metody `DeleteCategory` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-and-deleting-existing-binary-data-cs/_static/image8.png))

> [!NOTE]
> Po zakończeniu działania kreatora program Visual Studio może zażądać odświeżenia pól i kluczy, co spowoduje ponowne wygenerowanie pól formantów sieci Web danych. Wybierz pozycję nie, ponieważ wybranie opcji Tak spowoduje zastąpienie wszelkich dostosowanych dostosowań pól.

Element ObjectDataSource będzie teraz zawierać wartość właściwości `DeleteMethod`, a także `DeleteParameter`. Odwołaj, że podczas korzystania z kreatora w celu określenia metod program Visual Studio ustawia właściwość `OldValuesParameterFormatString` ObjectDataSource s na `original_{0}`, co powoduje problemy z wywołaniami metody Update i DELETE. W związku z tym Wyczyść tę właściwość całkowicie lub zresetuj ją do wartości domyślnej, `{0}`. Jeśli zachodzi potrzeba odświeżenia pamięci na tej właściwości ObjectDataSource, zobacz [Omówienie samouczka Wstawianie, aktualizowanie i usuwanie danych](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) .

Po ukończeniu działania kreatora i naprawieniu `OldValuesParameterFormatString`znaczniki deklaracyjne języka ObjectDataSource powinny wyglądać podobnie do następujących:

[!code-aspx[Main](updating-and-deleting-existing-binary-data-cs/samples/sample4.aspx)]

Po skonfigurowaniu elementu ObjectDataSource Dodaj możliwości usuwania do widoku GridView, zaznaczając pole wyboru Włącz usuwanie w tagu inteligentnym GridView. Spowoduje to dodanie CommandField do elementu GridView, którego właściwość `ShowDeleteButton` jest ustawiona na `true`.

[![włączyć obsługę usuwania w widoku GridView](updating-and-deleting-existing-binary-data-cs/_static/image5.gif)](updating-and-deleting-existing-binary-data-cs/_static/image9.png)

**Rysunek 5**. Włączanie obsługi usuwania w widoku GridView ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](updating-and-deleting-existing-binary-data-cs/_static/image10.png))

Poświęć chwilę na przetestowanie funkcji usuwania. Istnieje klucz obcy między tabelami `Products` `CategoryID` i `Categories` tabeli `CategoryID`, więc podczas próby usunięcia dowolnej z pierwszych ośmiu kategorii zostanie wyświetlony wyjątek naruszenia ograniczenia klucza obcego. Aby przetestować tę funkcję, Dodaj nową kategorię, dostarczając broszurę i obraz. Kategoria moje testy, pokazana na rysunku 6, zawiera plik broszury testowej o nazwie `Test.pdf` i obraz testowy. Rysunek 7 przedstawia widok GridView po dodaniu kategorii testów.

[![dodać kategorię testu z broszurą i obrazem](updating-and-deleting-existing-binary-data-cs/_static/image6.gif)](updating-and-deleting-existing-binary-data-cs/_static/image11.png)

**Ilustracja 6**. Dodawanie kategorii testów z broszurą i obrazem ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](updating-and-deleting-existing-binary-data-cs/_static/image12.png))

[![po wstawieniu kategorii testów zostanie ona wyświetlona w widoku GridView](updating-and-deleting-existing-binary-data-cs/_static/image7.gif)](updating-and-deleting-existing-binary-data-cs/_static/image13.png)

**Rysunek 7**. po wstawieniu kategorii testu zostanie ona wyświetlona w widoku GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-and-deleting-existing-binary-data-cs/_static/image14.png))

W programie Visual Studio Odśwież Eksplorator rozwiązań. W folderze `~/Brochures` powinien zostać wyświetlony nowy plik `Test.pdf` (zobacz rysunek 8).

Następnie kliknij łącze Usuń w wierszu kategorii testów, co spowoduje odświeżenie strony i `DeleteCategory` metody `CategoriesBLL` klasy s. Spowoduje to wywołanie metody DAL `Delete`, co spowoduje wysłanie odpowiedniej instrukcji `DELETE` do bazy danych. Dane są następnie ponownie powiązane z elementem GridView, a znaczniki są wysyłane z powrotem do klienta z nieobecną kategorią testu.

Mimo że przepływ pracy usuwania pomyślnie usunął rekord kategorii testu z tabeli `Categories`, plik broszury nie został usunięty z systemu plików serwera sieci Web. Odśwież Eksplorator rozwiązań i zobaczysz, że `Test.pdf` nadal znajduje się w folderze `~/Brochures`.

![Plik test. PDF nie został usunięty z systemu plików serwera sieci Web](updating-and-deleting-existing-binary-data-cs/_static/image8.gif)

**Ilustracja 8**. plik `Test.pdf` nie został usunięty z systemu plików serwera sieci Web

## <a name="step-5-removing-the-deleted-category-s-brochure-file"></a>Krok 5. Usuwanie usuniętego pliku broszury kategorii

Jednym z downsides przechowywania danych binarnych poza bazą danych jest to, że należy podjąć dodatkowe kroki w celu oczyszczenia tych plików po usunięciu skojarzonego rekordu bazy danych. Elementy GridView i ObjectDataSource dostarczają zdarzenia, które są uruchamiane zarówno przed, jak i po wykonaniu polecenia Delete. W rzeczywistości musimy utworzyć programy obsługi zdarzeń dla zdarzeń przednich i po akcji po stronie. Przed usunięciem rekordu `Categories` musimy określić jego ścieżkę pliku PDF, ale nie chcemy usunąć pliku PDF przed usunięciem kategorii, w przypadku gdy występuje jakiś wyjątek, a kategoria nie zostanie usunięta.

Zdarzenie GridView s [`RowDeleting`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx) wyzwalane przed wywołaniem polecenia usuwania elementu ObjectDataSource s, gdy [zdarzenie`RowDeleted`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleted.aspx) wyzwalane po. Utwórz programy obsługi zdarzeń dla tych dwóch zdarzeń przy użyciu następującego kodu:

[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample5.cs)]

W obsłudze zdarzeń `RowDeleting` `CategoryID` usuwany wiersz jest pobierany z kolekcji GridView `DataKeys`, do której można uzyskać dostęp w tym obsłudze zdarzeń za pośrednictwem kolekcji `e.Keys`. Następnie `CategoriesBLL` klasie s `GetCategoryByCategoryID(categoryID)` zostanie wywołana w celu zwrócenia informacji o usuwanym rekordzie. Jeśli zwracany obiekt `CategoriesDataRow` ma wartość, która nie jest`NULL``BrochurePath`, jest przechowywany w zmiennej strony `deletedCategorysPdfPath` tak, aby plik mógł zostać usunięty w programie obsługi zdarzeń `RowDeleted`.

> [!NOTE]
> Zamiast pobierać `BrochurePath` szczegóły dla rekordu `Categories`, który jest usuwany w procedurze obsługi zdarzeń `RowDeleting`, możemy również dodać `BrochurePath` do właściwości GridView s `DataKeyNames` i uzyskać dostęp do rekordu s przy użyciu kolekcji `e.Keys`. Wykonanie tej operacji spowodowałoby nieco zwiększenie rozmiaru stanu widoku GridView, ale zmniejszy ilość wymaganego kodu i zapisze podróż do bazy danych.

Po wywołaniu polecenia usuwania bazowego elementu ObjectDataSource s `RowDeleted` procedury obsługi zdarzeń w widoku GridView. Jeśli nie ma żadnych wyjątków w usuwaniu danych, a istnieje wartość `deletedCategorysPdfPath`, plik PDF zostanie usunięty z systemu plików. Należy zauważyć, że ten dodatkowy kod nie jest wymagany do oczyszczenia danych binarnych kategorii skojarzonych z jego obrazem. To nie, ponieważ dane obrazu są przechowywane bezpośrednio w bazie danych, więc usunięcie wiersza `Categories` spowoduje również usunięcie danych z obrazu.

Po dodaniu dwóch programów obsługi zdarzeń ponownie uruchom ten przypadek testowy. Usunięcie kategorii spowoduje również usunięcie skojarzonego z nim pliku PDF.

Aktualizowanie istniejącego rekordu s skojarzonych danych binarnych zapewnia interesujące wyzwania. Pozostała część tego samouczka służy do dodawania funkcji aktualizacji do broszury i obrazu. Krok 6 Eksplorowanie technik aktualizowania informacji o broszurze, gdy krok 7 wygląda na aktualizowanie obrazu.

## <a name="step-6-updating-a-category-s-brochure"></a>Krok 6. aktualizowanie broszury kategorii s

Zgodnie z opisem w temacie [Omówienie wstawiania, aktualizowania i usuwania danych](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) , widok GridView oferuje wbudowaną obsługę edycji na poziomie wiersza, która może być implementowana przez znacznik wyboru, jeśli jego bazowe źródło danych jest odpowiednio skonfigurowane. Obecnie `CategoriesDataSource` element ObjectDataSource nie został jeszcze skonfigurowany do uwzględniania obsługi aktualizacji, dlatego Dodaj go do programu.

Kliknij link Konfiguruj źródło danych z Kreatora ObjectDataSource s i przejdź do drugiego kroku. Ze względu na `DataObjectMethodAttribute` używane w `CategoriesBLL`, lista rozwijana aktualizacji powinna być automatycznie wypełniana `UpdateCategory` Przeciążenie, które akceptuje cztery parametry wejściowe (dla wszystkich kolumn, ale `Picture`). Zmień ten element tak, aby używał przeciążenia z pięcioma parametrami.

[![skonfigurować element ObjectDataSource do używania metody UpdateCategory, która zawiera parametr dla obrazu](updating-and-deleting-existing-binary-data-cs/_static/image9.gif)](updating-and-deleting-existing-binary-data-cs/_static/image15.png)

**Rysunek 9**. Konfigurowanie elementu ObjectDataSource do używania metody `UpdateCategory`, która zawiera parametr dla `Picture` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](updating-and-deleting-existing-binary-data-cs/_static/image16.png))

Element ObjectDataSource będzie teraz zawierać wartość właściwości `UpdateMethod` oraz odpowiednie `UpdateParameter` s. Jak wskazano w kroku 4, program Visual Studio ustawia właściwość `OldValuesParameterFormatString` ObjectDataSource s na `original_{0}` podczas korzystania z Kreatora konfiguracji źródła danych. Spowoduje to problemy z wywołaniami metody Update i DELETE. W związku z tym Wyczyść tę właściwość całkowicie lub zresetuj ją do wartości domyślnej, `{0}`.

Po ukończeniu działania kreatora i naprawieniu `OldValuesParameterFormatString`znacznik deklaratywny elementu ObjectDataSource s powinien wyglądać następująco:

[!code-aspx[Main](updating-and-deleting-existing-binary-data-cs/samples/sample6.aspx)]

Aby włączyć wbudowane funkcje edytowania GridView, zaznacz opcję Włącz edytowanie w tagu inteligentnym GridView. Spowoduje to ustawienie właściwości CommandField s `ShowEditButton` na `true`, co powoduje dodanie przycisku edycji (oraz przyciski Aktualizuj i Anuluj dla edytowanego wiersza).

[![skonfigurować widok GridView do obsługi edycji](updating-and-deleting-existing-binary-data-cs/_static/image10.gif)](updating-and-deleting-existing-binary-data-cs/_static/image17.png)

**Ilustracja 10**. Konfigurowanie widoku GridView do obsługi edycji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-and-deleting-existing-binary-data-cs/_static/image18.png))

Odwiedź stronę za pomocą przeglądarki i kliknij jeden z przycisków edycji wiersza. `CategoryName` i `Description` BoundFields są renderowane jako pola tekstowe. `BrochurePath` TemplateField nie ma `EditItemTemplate`, dlatego nadal pokazuje `ItemTemplate` łącze do broszury. `Picture` ImageField renderuje jako pole tekstowe, którego właściwość `Text` ma przypisaną wartość ImageField s `DataImageUrlField` w tym przypadku `CategoryID`.

[![GridView nie ma interfejsu edycji dla BrochurePath](updating-and-deleting-existing-binary-data-cs/_static/image11.gif)](updating-and-deleting-existing-binary-data-cs/_static/image19.png)

**Ilustracja 11**. element GridView nie ma interfejsu edycji dla `BrochurePath` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](updating-and-deleting-existing-binary-data-cs/_static/image20.png))

## <a name="customizing-thebrochurepaths-editing-interface"></a>Dostosowywanie interfejsu edycji`BrochurePath`s

Musimy utworzyć interfejs edycji dla `BrochurePath` TemplateField, który umożliwia użytkownikowi:

- Pozostaw broszurę kategorii s jako-is,
- Zaktualizuj broszurę kategorii, przekazując nową broszurę lub
- Całkowicie Usuń broszurę kategorii s (w przypadku, gdy Kategoria nie ma już skojarzonej broszury).

Konieczne jest również zaktualizowanie interfejsu `Picture` ImageField (Edycja), ale w kroku 7 zostanie wyświetlony ten interfejs.

W tagu inteligentnym GridView kliknij łącze Edytuj szablony i wybierz `BrochurePath` TemplateField s `EditItemTemplate` z listy rozwijanej. Dodaj kontrolkę sieci Web RadioButtonList do tego szablonu, ustawiając jej Właściwość `ID` na `BrochureOptions` i jej Właściwość `AutoPostBack` na `true`. Na okno Właściwości kliknij wielokropek we właściwości `Items`, co spowoduje wyświetlenie edytora kolekcji `ListItem`. Dodaj następujące trzy opcje, odpowiednio `Value` s 1, 2 i 3:

- Użyj bieżącej broszury
- Usuń bieżącą broszurę
- Przekaż nową broszurę

Ustaw wartość właściwości First `ListItem` s `Selected` na `true`.

![Dodaj trzy listy ListItems do RadioButtonList](updating-and-deleting-existing-binary-data-cs/_static/image12.gif)

**Rysunek 12**. dodanie trzech `ListItem` s do RadioButtonList

Poniżej RadioButtonList Dodaj kontrolkę FileUpload o nazwie `BrochureUpload`. Ustaw jej Właściwość `Visible` na `false`.

[![dodać kontrolkę RadioButtonList i FileUpload do EditItemTemplate](updating-and-deleting-existing-binary-data-cs/_static/image13.gif)](updating-and-deleting-existing-binary-data-cs/_static/image21.png)

**Ilustracja 13**. Dodawanie kontrolki RadioButtonList i FileUpload do `EditItemTemplate` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](updating-and-deleting-existing-binary-data-cs/_static/image22.png))

Ta RadioButtonList udostępnia trzy opcje dla użytkownika. Pomysł polega na tym, że kontrolka FileUpload będzie wyświetlana tylko wtedy, gdy jest zaznaczona Ostatnia opcja, Przekaż nową broszurę. Aby to osiągnąć, Utwórz procedurę obsługi zdarzeń dla zdarzenia `SelectedIndexChanged` RadioButtonList s i Dodaj następujący kod:

[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample7.cs)]

Ponieważ kontrolki RadioButtonList i FileUpload znajdują się w obrębie szablonu, musimy napisać bit kodu, aby programowo uzyskać dostęp do tych kontrolek. Procedura obsługi zdarzeń `SelectedIndexChanged` została przeniesiona odwołaniem RadioButtonList w `sender` parametr wejściowy. Aby uzyskać formant FileUpload, musimy uzyskać kontrolkę nadrzędną RadioButtonList i użyć z niej metody `FindControl("controlID")`. Gdy mamy odwołanie do kontrolek RadioButtonList i FileUpload, właściwość FileUpload Control `Visible` jest ustawiona na `true` tylko wtedy, gdy RadioButtonList s `SelectedValue` równa 3, która jest `Value` do przekazywania nowej broszury `ListItem`.

Po dokonaniu tego kodu Poświęć chwilę na przetestowanie interfejsu edycji. Kliknij przycisk Edytuj dla wiersza. Początkowo należy wybrać opcję Użyj bieżącej broszury. Zmiana wybranego indeksu spowoduje odświeżenie. Jeśli zaznaczono trzecią opcję, zostanie wyświetlona kontrolka FileUpload. w przeciwnym razie jest ukryta. Ilustracja 14 przedstawia interfejs edycji po pierwszym kliknięciu przycisku Edytuj; Rysunek 15 pokazuje interfejs po wybraniu opcji Przekaż nową broszurę.

[![początkowo zaznaczona jest opcja Użyj bieżącej broszury](updating-and-deleting-existing-binary-data-cs/_static/image14.gif)](updating-and-deleting-existing-binary-data-cs/_static/image23.png)

**Rysunek 14**. Początkowo zaznaczona jest opcja Użyj bieżącej broszury ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-and-deleting-existing-binary-data-cs/_static/image24.png))

[![wybranie opcji Przekaż nową broszurę Wyświetla formant FileUpload](updating-and-deleting-existing-binary-data-cs/_static/image15.gif)](updating-and-deleting-existing-binary-data-cs/_static/image25.png)

**Ilustracja 15**. wybranie opcji Przekaż nową broszurę powoduje wyświetlenie kontrolki FileUpload ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](updating-and-deleting-existing-binary-data-cs/_static/image26.png))

## <a name="saving-the-brochure-file-and-updating-thebrochurepathcolumn"></a>Zapisywanie pliku broszury i aktualizowanie kolumny`BrochurePath`

Po kliknięciu przycisku aktualizacji GridView s zostanie wyzwolone jego zdarzenie `RowUpdating`. Polecenie ObjectDataSource s Update jest wywoływane, a następnie uruchamiane jest zdarzenie GridView `RowUpdated`. Podobnie jak w przypadku usuwania przepływu pracy, musimy utworzyć obsługę zdarzeń dla obu tych zdarzeń. W obsłudze zdarzeń `RowUpdating` musimy określić, jakie działania należy podjąć w oparciu o `SelectedValue` `BrochureOptions` RadioButtonList:

- Jeśli `SelectedValue` wynosi 1, chcemy nadal korzystać z tego samego ustawienia `BrochurePath`. W związku z tym musimy ustawić parametr ObjectDataSource s `brochurePath` na istniejącą wartość `BrochurePath` aktualizowanego rekordu. Parametr `brochurePath` ObjectDataSource s można ustawić przy użyciu `e.NewValues["brochurePath"] = value`.
- Jeśli `SelectedValue` jest równa 2, wówczas chcemy ustawić wartość rekordu s `BrochurePath` na `NULL`. Można to osiągnąć przez ustawienie parametru `brochurePath` ObjectDataSource s na `Nothing`, co spowoduje, że w instrukcji `UPDATE` zostanie użyta `NULL` baza danych. Jeśli istnieje plik broszury, który jest usuwany, musimy usunąć istniejący plik. Jednak chcemy to zrobić tylko wtedy, gdy aktualizacja zakończy się bez zgłaszania wyjątku.
- Jeśli `SelectedValue` to 3, chcemy upewnić się, że użytkownik przekazał plik PDF, a następnie zapisze go w systemie plików i zaktualizuj wartość kolumny `BrochurePath` rekordów. Ponadto jeśli istnieje plik broszury, który jest zastępowany, musimy usunąć poprzedni plik. Jednak chcemy to zrobić tylko wtedy, gdy aktualizacja zakończy się bez zgłaszania wyjątku.

Kroki, które należy wykonać, gdy RadioButtonList `SelectedValue` jest 3, są niemal identyczne jak te używane przez program obsługi zdarzeń `ItemInserting` DetailsView s. Ta procedura obsługi zdarzeń jest wykonywana po dodaniu nowego rekordu kategorii z kontrolki DetailsView, która została dodana w [poprzednim samouczku](including-a-file-upload-option-when-adding-a-new-record-cs.md). W związku z tym behooves nam się to w oddzielnym metodzie. W odniesieniu do dwóch metod zostały przesunięte typowe funkcje:

- `ProcessBrochureUpload(FileUpload, out bool)` akceptuje jako dane wejściowe wystąpienia formantu FileUpload oraz wyjściową wartość logiczną określającą, czy operacja usuwania lub edycji powinna być wykonywana czy powinna zostać anulowana z powodu błędu walidacji. Ta metoda zwraca ścieżkę do zapisanego pliku lub `null`, jeśli żaden plik nie został zapisany.
- `DeleteRememberedBrochurePath` usuwa plik określony przez ścieżkę w zmiennej strony `deletedCategorysPdfPath` Jeśli `deletedCategorysPdfPath` nie `null`.

Poniżej znajduje się kod dla tych dwóch metod. Należy pamiętać o podobieństwie między `ProcessBrochureUpload` i obsługą zdarzeń DetailsView s `ItemInserting` z poprzedniego samouczka. W tym samouczku Zaktualizowaliśmy procedury obsługi zdarzeń DetailsView s w celu korzystania z tych nowych metod. Pobierz kod skojarzony z tym samouczkiem, aby zobaczyć modyfikacje obsługi zdarzeń DetailsView s.

[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample8.cs)]

Procedury obsługi zdarzeń `RowUpdating` GridView i `RowUpdated` używają metod `ProcessBrochureUpload` i `DeleteRememberedBrochurePath`, jak pokazano w poniższym kodzie:

[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample9.cs)]

Zwróć uwagę, jak program obsługi zdarzeń `RowUpdating` korzysta z serii instrukcji warunkowych, aby wykonać odpowiednią akcję na podstawie wartości właściwości `BrochureOptions` RadioButtonList s `SelectedValue`.

Przy użyciu tego kodu można edytować kategorię i korzystać z jej bieżącej broszury, korzystać z broszury lub przekazać nową. Przejdź dalej i wypróbuj. Ustaw punkty przerwania w `RowUpdating` i `RowUpdated` obsługi zdarzeń, aby uzyskać sens przepływu pracy.

## <a name="step-7-uploading-a-new-picture"></a>Krok 7. przekazywanie nowego obrazu

Interfejs ImageField `Picture` w trybie edycji jest renderowany jako pole tekstowe wypełnione wartością z właściwości `DataImageUrlField`. Podczas edytowania przepływu pracy, element GridView przekazuje parametr do elementu ObjectDataSource z parametrem s Name wartości właściwości ImageField `DataImageUrlField` i parametr s wartości wprowadzonej w polu tekstowym w interfejsie edycji. To zachowanie jest odpowiednie, gdy obraz jest zapisywany jako plik w systemie plików, a `DataImageUrlField` zawiera pełny adres URL obrazu. W takim przypadku interfejs edytowania wyświetla adres URL obrazu s w polu tekstowym, które użytkownik może zmienić i zapisać z powrotem w bazie danych. Przyznany, ten domyślny interfejs nie zezwala użytkownikowi na przekazywanie nowego obrazu, ale pozwala im zmienić adres URL obrazu z bieżącej wartości na inny. Jednak w tym samouczku interfejs edycji ImageField s nie wystarcza, ponieważ `Picture` dane binarne są przechowywane bezpośrednio w bazie danych, a właściwość `DataImageUrlField` zawiera tylko `CategoryID`.

Aby lepiej zrozumieć, co się dzieje w naszym samouczku, gdy użytkownik edytuje wiersz za pomocą ImageField, rozważmy następujący przykład: użytkownik edytuje wiersz z `CategoryID` 10, co spowoduje, że `Picture` ImageField do renderowania jako pole tekstowe z wartością 10. Załóżmy, że użytkownik zmienia wartość w tym polu tekstowym na 50, a następnie klika przycisk Aktualizuj. Następuje ogłaszanie zwrotne, a GridView początkowo tworzy parametr o nazwie `CategoryID` o wartości 50. Jednak przed wysłaniem przez GridView tego parametru (i parametrów `CategoryName` i `Description`) dodaje on wartości z kolekcji `DataKeys`. W związku z tym zastępuje parametr `CategoryID` z bieżącym wierszem `CategoryID` wartość podstawowa, 10. W skrócie interfejs edytowania ImageField nie ma wpływu na przepływ pracy edytowania dla tego samouczka, ponieważ nazwy właściwości ImageField s `DataImageUrlField` i siatki s `DataKey` wartości są takie same.

Chociaż ImageField ułatwia wyświetlanie obrazu na podstawie danych bazy danych, nie chcemy podawać pola tekstowego w interfejsie edycji. Zamiast tego chcemy oferować formant FileUpload, którego użytkownik końcowy może użyć do zmiany obrazu kategorii. W przeciwieństwie do wartości `BrochurePath`, dla samouczków wprowadziliśmy, że każda kategoria musi mieć obraz. W związku z tym nie musimy pozwolić użytkownikowi na wskazanie, że nie ma żadnych powiązanych zdjęć, użytkownik może albo przekazać nowy obraz, albo pozostawić bieżący obraz.

Aby dostosować interfejs edycji ImageField, musimy przekonwertować go na TemplateField. W tagu inteligentnym GridView kliknij łącze Edytuj kolumny, wybierz ImageField, a następnie kliknij przycisk Konwertuj to pole na link TemplateField.

![Konwertuj ImageField na TemplateField](updating-and-deleting-existing-binary-data-cs/_static/image16.gif)

**Rysunek 16**. konwertowanie ImageField na TemplateField

Konwersja ImageField na TemplateField w ten sposób generuje TemplateField z dwoma szablonami. Jak przedstawiono w poniższej składni deklaracyjnej, `ItemTemplate` zawiera formant sieci Web obrazu, którego właściwość `ImageUrl` jest przypisana przy użyciu składni DataBinding na podstawie właściwości `DataImageUrlField` i `DataImageUrlFormatString` ImageField. `EditItemTemplate` zawiera pole tekstowe, którego właściwość `Text` jest powiązana z wartością określoną przez właściwość `DataImageUrlField`.

[!code-aspx[Main](updating-and-deleting-existing-binary-data-cs/samples/sample10.aspx)]

Musimy zaktualizować `EditItemTemplate`, aby użyć formantu FileUpload. Z poziomu tagu inteligentnego GridView kliknij link Edytuj szablony, a następnie wybierz `Picture` TemplateField s `EditItemTemplate` z listy rozwijanej. W szablonie powinna zostać wyświetlona wartość pola tekstowego Usuń to. Następnie przeciągnij kontrolkę FileUpload z przybornika do szablonu, ustawiając jej `ID` na `PictureUpload`. Dodaj również tekst, aby zmienić obraz kategorii, określ nowy obraz. Aby zachować zdjęcie kategorii s, pozostaw to pole puste również do szablonu.

[![dodać kontrolkę FileUpload do EditItemTemplate](updating-and-deleting-existing-binary-data-cs/_static/image17.gif)](updating-and-deleting-existing-binary-data-cs/_static/image27.png)

**Ilustracja 17**. Dodawanie kontrolki FileUpload do `EditItemTemplate` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-and-deleting-existing-binary-data-cs/_static/image28.png))

Po dostosowaniu interfejsu edycji Przejrzyj postęp w przeglądarce. Podczas wyświetlania wiersza w trybie tylko do odczytu obraz kategorii s jest pokazywany tak jak wcześniej, ale kliknięcie przycisku Edytuj powoduje renderowanie kolumny obrazu jako tekstu z kontrolką FileUpload.

[![interfejs edycji zawiera kontrolkę FileUpload](updating-and-deleting-existing-binary-data-cs/_static/image18.gif)](updating-and-deleting-existing-binary-data-cs/_static/image29.png)

**Ilustracja 18**. interfejs edycji zawiera kontrolkę FileUpload ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](updating-and-deleting-existing-binary-data-cs/_static/image30.png))

Odwołaj, że element ObjectDataSource jest skonfigurowany tak, aby wywoływał metodę `CategoriesBLL` klasy s `UpdateCategory`, która akceptuje jako dane wejściowe danych binarnych dla obrazu jako tablicę `byte`. Jeśli ta tablica ma `null` wartość, jest wywoływana alternatywna `UpdateCategory` Przeciążenie, która wystawia `UPDATE` instrukcję SQL, która nie modyfikuje kolumny `Picture`, w związku z tym pozostawiając bieżący obraz kategorii s nienaruszony. W związku z tym w GridView s `RowUpdating` obsługi zdarzeń musi programistycznie odwoływać się do `PictureUpload` formantu FileUpload i określić, czy plik został przekazany. Jeśli nikt nie został przekazany, *nie* chcemy określać wartości parametru `picture`. Z drugiej strony, jeśli plik został przekazany w `PictureUpload` formantu FileUpload, chcemy upewnić się, że jest to plik JPG. Jeśli tak, można wysłać zawartość binarną do elementu ObjectDataSource za pomocą parametru `picture`.

Podobnie jak w przypadku kodu używanego w kroku 6, większość kodu wymaganego w tym miejscu już istnieje w programie obsługi zdarzeń `ItemInserting` DetailsView. W związku z tym, chcę, aby nowa metoda została przekształcona w nową metodę, `ValidPictureUpload`i Zaktualizowano procedurę obsługi zdarzeń `ItemInserting` do korzystania z tej metody.

Dodaj następujący kod na początku procedury obsługi zdarzeń `RowUpdating` GridView. Należy pamiętać, że ten kod jest wcześniejszy niż kod, który zapisuje plik broszury, ponieważ nie chcemy zapisać broszury w systemie plików serwera sieci Web, jeśli zostanie przekazany nieprawidłowy plik obrazu.

[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample11.cs)]

Metoda `ValidPictureUpload(FileUpload)` przyjmuje jako jedyny parametr wejściowy formant FileUpload i sprawdza przekazane rozszerzenie pliku, aby upewnić się, że przekazany plik jest w formacie JPG; jest wywoływana tylko w przypadku przekazania pliku obrazu. Jeśli plik nie zostanie przekazany, parametr obrazu nie jest ustawiony i w związku z tym używa jego wartości domyślnej `null`. Jeśli zdjęcie zostało przekazane i `ValidPictureUpload` zwraca `true`, `picture` parametr przypisuje dane binarne przekazanego obrazu; Jeśli metoda zwraca `false`, przepływ pracy aktualizacji zostanie anulowany, a program obsługi zdarzeń zakończył działanie.

Kod metody `ValidPictureUpload(FileUpload)`, który został przeszacowany z procedury obsługi zdarzeń DetailsView s `ItemInserting`:

[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample12.cs)]

## <a name="step-8-replacing-the-original-categories-pictures-with-jpgs"></a>Krok 8. zastępowanie oryginalnych obrazów kategorii JPGs

Odwołaj się, że oryginalne osiem kategorii zdjęć są plikami mapy bitowej opakowanymi w nagłówku OLE. Teraz, gdy dodaliśmy możliwość edytowania istniejącego rekordu s, poświęć chwilę, aby zamienić te mapy bitowe na JPGs. Jeśli chcesz kontynuować korzystanie z bieżących obrazów kategorii, możesz je przekonwertować na JPGs, wykonując następujące czynności:

1. Zapisz obrazy mapy bitowej na dysku twardym. Odwiedź stronę `UpdatingAndDeleting.aspx` w przeglądarce i dla każdej z pierwszych ośmiu kategorii, kliknij prawym przyciskiem myszy obraz i wybierz polecenie zapisania obrazu.
2. Otwórz obraz w wybranym edytorze obrazu. Możesz na przykład użyć programu Microsoft Paint.
3. Zapisz mapę bitową jako obraz JPG.
4. Zaktualizuj obraz kategorii w interfejsie edycji przy użyciu pliku JPG.

Po przeprowadzeniu edycji kategorii i przekazaniu obrazu JPG obraz nie będzie renderowany w przeglądarce, ponieważ strona `DisplayCategoryPicture.aspx` usuwa pierwsze 78 bajtów z zdjęć pierwszych ośmiu kategorii. Rozwiąż ten problem, usuwając kod, który wykonuje odcięcie nagłówka OLE. Po wykonaniu tej czynności program obsługi zdarzeń `DisplayCategoryPicture.aspx``Page_Load` powinien mieć tylko następujący kod:

[!code-vb[Main](updating-and-deleting-existing-binary-data-cs/samples/sample13.vb)]

> [!NOTE]
> Użycie i edytowanie interfejsów na stronie `UpdatingAndDeleting.aspx` może zwiększyć liczbę wydajnych zadań. `CategoryName` i `Description` BoundFields w widoku DetailsView i GridView powinny zostać przekonwertowane na używanie TemplateField. Ponieważ `CategoryName` nie zezwala na wartości `NULL`, należy dodać RequiredFieldValidator. I pole tekstowe `Description` powinno być prawdopodobnie konwertowane w wielowierszowym polu tekstowym. Opuszczam te działania, które są dla Ciebie takie same.

## <a name="summary"></a>Podsumowanie

Ten samouczek kończy pracę z danymi binarnymi. W tym samouczku i poprzednich trzech zadaliśmy, jak dane binarne mogą być przechowywane w systemie plików lub bezpośrednio w bazie danych. Użytkownik dostarcza dane binarne do systemu przez wybranie pliku z dysku twardego i przekazanie go do serwera sieci Web, w którym można go zapisać w systemie plików lub wstawić do bazy danych programu. ASP.NET 2,0 zawiera kontrolkę FileUpload, która zapewnia takiemu interfejsowi łatwe jak przeciąganie i upuszczanie. Jednak zgodnie z samouczkiem [przekazywanie plików](uploading-files-cs.md) formant FileUpload jest dobrze dostosowany do stosunkowo małych przeciążeń plików, najlepiej nie przekroczy megabajtów. Szczegółowo omówiono, jak skojarzyć przekazane dane z źródłowym modelem danych, jak również edytować i usuwać dane binarne z istniejących rekordów.

Nasz następny zestaw samouczków eksploruje różne techniki buforowania. Buforowanie zapewnia metodę poprawy ogólnej wydajności aplikacji, pobierając wyniki z kosztownych operacji i przechowując je w lokalizacji, do której można uzyskać szybszy dostęp.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzent potencjalnych klientów dla tego samouczka został Teresa Murphy. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](including-a-file-upload-option-when-adding-a-new-record-cs.md)
> [dalej](uploading-files-vb.md)

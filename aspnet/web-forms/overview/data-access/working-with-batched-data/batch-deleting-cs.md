---
uid: web-forms/overview/data-access/working-with-batched-data/batch-deleting-cs
title: Usuwanie partii (C#) | Microsoft Docs
author: rick-anderson
description: Dowiedz się, jak usunąć wiele rekordów bazy danych w ramach jednej operacji. W warstwie interfejsu użytkownika została utworzona po rozbudowanym widoku GridView utworzonym we wcześniejszej tut...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: ac6916d0-a5ab-4218-9760-7ba9e72d258c
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: ed832c38b4972f440ab64c141e29c85f0a9df920
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78589531"
---
# <a name="batch-deleting-c"></a>Usuwanie w partiach (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_65_CS.zip) lub [Pobierz plik PDF](batch-deleting-cs/_static/datatutorial65cs1.pdf)

> Dowiedz się, jak usunąć wiele rekordów bazy danych w ramach jednej operacji. W warstwie interfejsu użytkownika została utworzona po rozbudowanym widoku GridView utworzonym w poprzednim samouczku. W warstwie dostępu do danych zawijamy wiele operacji usuwania w ramach transakcji, aby upewnić się, że wszystkie operacje usuwania zakończyły się powodzeniem lub zostaną wycofane wszystkie usunięcia.

## <a name="introduction"></a>Wprowadzenie

W [poprzednim samouczku](batch-updating-cs.md) przedstawiono sposób tworzenia interfejsu edycji partii przy użyciu w pełni edytowanego widoku GridView. W sytuacjach, w których użytkownicy często edytują wiele rekordów jednocześnie, interfejs edytowania partii będzie wymagał znacznie mniejszego stanu ogłaszania zwrotnego i przełączania kontekstu klawiatury na klawiaturę, a tym samym podwyższenie wydajności użytkowników końcowych. Ta technika jest podobnie przydatna w przypadku stron, na których często użytkownicy mogą usuwać wiele rekordów w jednym miejscu.

Każda osoba, która użyła klienta poczty e-mail w trybie online, już zna jeden z najczęstszych interfejsów usuwania usługi Batch: pole wyboru w każdym wierszu siatki z odpowiednim przyciskiem Usuń wszystkie zaznaczone elementy (patrz rysunek 1). Ten samouczek jest raczej niezwięzły, ponieważ w poprzednich samouczkach została już wykonana cała twarda część pracy, w której można utworzyć interfejs oparty na sieci Web i metodę usuwania serii rekordów jako pojedynczej operacji niepodzielnej. W samouczku [Dodawanie pola wyboru w kolumnie GridView](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs.md) został utworzony widok GridView z kolumną pól wyboru i w [modyfikacjach zawijania bazy danych w ramach instrukcji Transaction](wrapping-database-modifications-within-a-transaction-cs.md) utworzyliśmy metodę w logiki biznesowej, która mogłaby używać transakcji do usuwania `List<T>` `ProductID` wartości. W tym samouczku będziemy kompilować i scalać nasze poprzednie środowiska w celu utworzenia przykładowego programu do usuwania partii.

[![każdy wiersz zawiera pole wyboru](batch-deleting-cs/_static/image1.gif)](batch-deleting-cs/_static/image1.png)

**Rysunek 1**: każdy wiersz zawiera pole wyboru ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](batch-deleting-cs/_static/image2.png))

## <a name="step-1-creating-the-batch-deleting-interface"></a>Krok 1. Tworzenie interfejsu usuwania wsadowego

Ze względu na to, że został już utworzony Interfejs usuwania usługi Batch w ramach samouczka [Dodawanie tabeli w widoku GridView](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs.md) , można po prostu skopiować go do `BatchDelete.aspx` zamiast tworzyć go od podstaw. Zacznij od otwarcia strony `BatchDelete.aspx` w folderze `BatchData` i stronie `CheckBoxField.aspx` w folderze `EnhancedGridView`. Na stronie `CheckBoxField.aspx` przejdź do widoku źródła i skopiuj znaczniki między tagami `<asp:Content>`, jak pokazano na rysunku 2.

[![skopiować deklaratywne znaczniki CheckBoxField. aspx do schowka](batch-deleting-cs/_static/image2.gif)](batch-deleting-cs/_static/image3.png)

**Rysunek 2**. Skopiuj deklaratywne znaczniki `CheckBoxField.aspx` do schowka ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](batch-deleting-cs/_static/image4.png))

Następnie przejdź do widoku źródła w `BatchDelete.aspx` i wklej zawartość schowka w tagach `<asp:Content>`. Kopiowanie i wklejanie kodu z klasy powiązanej z kodem w `CheckBoxField.aspx.cs` do w ramach klasy z kodem związanym w `BatchDelete.aspx.cs` (`DeleteSelectedProducts` przycisk `Click` programu obsługi zdarzeń, metody `ToggleCheckState` oraz obsługi zdarzeń `Click` dla przycisków `CheckAll` i `UncheckAll`). Po skopiowaniu zawartości przez tę zawartość Klasa `BatchDelete.aspx` s z kodem związanym ze stroną powinna zawierać następujący kod:

[!code-csharp[Main](batch-deleting-cs/samples/sample1.cs)]

Po skopiowaniu do deklaratywnego znacznika i kodu źródłowego Poświęć chwilę na przetestowanie `BatchDelete.aspx`, wyświetlając go za pośrednictwem przeglądarki. Powinien pojawić się widok GridView zawierający pierwsze dziesięć produktów w widoku GridView z każdym wierszem zawierającym nazwę produktu, kategorię i cenę wraz z checkboxem. Powinny istnieć trzy przyciski: Zaznacz wszystko, usuń zaznaczenie wszystkich i Usuń wybrane produkty. Kliknięcie przycisku Zaznacz wszystko powoduje zaznaczenie wszystkich pól wyboru, natomiast zaznaczenie pola Wyczyść wszystkie czyści wszystkie pola wyboru. Kliknięcie przycisku Usuń wybrane produkty powoduje wyświetlenie komunikatu zawierającego listę wartości `ProductID` wybranych produktów, ale nie powoduje usunięcia tych produktów.

[![interfejs z CheckBoxField. aspx został przeniesiony do BatchDeleting. aspx](batch-deleting-cs/_static/image3.gif)](batch-deleting-cs/_static/image5.png)

**Rysunek 3**: interfejs z `CheckBoxField.aspx` został przeniesiony do `BatchDeleting.aspx` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](batch-deleting-cs/_static/image6.png))

## <a name="step-2-deleting-the-checked-products-using-transactions"></a>Krok 2. Usuwanie zaewidencjonowanych produktów przy użyciu transakcji

Gdy interfejs usuwania wsadowego został pomyślnie skopiowany do `BatchDeleting.aspx`, wszystko to jest w celu zaktualizowania kodu tak, aby przycisk Usuń wybrane produkty usuwa zaznaczone produkty przy użyciu metody `DeleteProductsWithTransaction` w klasie `ProductsBLL`. Ta metoda dodana w ramach samouczka dotyczącego [zawijania bazy danych](wrapping-database-modifications-within-a-transaction-cs.md) , która akceptuje `List<T>` wartości `ProductID` i usuwa wszystkie odpowiednie `ProductID` w zakresie transakcji.

`DeleteSelectedProducts` przycisku s `Click` obsługi zdarzeń używa obecnie następującej pętli `foreach` do iteracji przez każdy wiersz GridView:

[!code-csharp[Main](batch-deleting-cs/samples/sample2.cs)]

Dla każdego wiersza formant sieci Web CheckBox `ProductSelector` jest wywoływany programowo. Jeśli ta opcja jest zaznaczona, wiersz s `ProductID` jest pobierany z kolekcji `DataKeys` i Właściwość `DeleteResults` etykieta s `Text` zostanie zaktualizowana tak, aby zawierała komunikat informujący o tym, że wiersz został wybrany do usunięcia.

Powyższy kod nie usuwa żadnych rekordów, ponieważ wywołanie metody `Delete` `ProductsBLL` klasy s jest oznaczone jako komentarz. Czy ta logika usuwania ma zostać zastosowana, kod usunie produkty, ale nie w obrębie operacji niepodzielnej. Oznacza to, że jeśli pierwsze kilka usunięć w sekwencji zakończyło się pomyślnie, ale późniejsza z nich zakończyło się niepowodzeniem (prawdopodobnie z powodu naruszenia ograniczenia klucza obcego), zostanie zgłoszony wyjątek, ale te produkty już usunięte pozostaną usunięte.

Aby zapewnić niepodzielność, musimy użyć metody `DeleteProductsWithTransaction` `ProductsBLL` Class. Ponieważ ta metoda akceptuje listę wartości `ProductID`, musimy najpierw skompilować tę listę z siatki, a następnie przekazać ją jako parametr. Najpierw tworzymy wystąpienie `List<T>` typu `int`. W pętli `foreach` musimy dodać wybrane produkty `ProductID` wartości do tej `List<T>`. Gdy pętla ta `List<T>` musi zostać przeniesiona do metody `DeleteProductsWithTransaction` `ProductsBLL` Class s. Zaktualizuj `Click` `DeleteSelectedProducts` przycisk programu obsługi zdarzeń przy użyciu następującego kodu:

[!code-csharp[Main](batch-deleting-cs/samples/sample3.cs)]

Zaktualizowany kod tworzy `List<T>` typu `int` (`productIDsToDelete`) i wypełnia go wartościami `ProductID` do usunięcia. Po pętli `foreach`, jeśli istnieje co najmniej jeden produkt, Metoda `ProductsBLL` klasy s `DeleteProductsWithTransaction` jest wywoływana i przeszedł tę listę. Zostanie również wyświetlona etykieta `DeleteResults` i dane przepowiązane z elementem GridView (w taki sposób, że nowo usunięte rekordy nie będą już wyświetlane jako wiersze w siatce).

Rysunek 4 przedstawia widok GridView po wybraniu kilku wierszy do usunięcia. Rysunek 5 pokazuje ekran zaraz po kliknięciu przycisku Usuń wybrane produkty. Zwróć uwagę, że na rysunku 5 wartości `ProductID` usuniętych rekordów są wyświetlane w etykiecie poniżej widoku GridView i te wiersze nie znajdują się już w widoku GridView.

[![zostaną usunięte wybrane produkty](batch-deleting-cs/_static/image4.gif)](batch-deleting-cs/_static/image7.png)

**Ilustracja 4**. wybrane produkty zostaną usunięte (kliknij,[Aby wyświetlić obraz o pełnym rozmiarze](batch-deleting-cs/_static/image8.png))

[![wartości ProductID usuniętych produktów są wyświetlane poniżej widoku GridView](batch-deleting-cs/_static/image5.gif)](batch-deleting-cs/_static/image9.png)

**Rysunek 5**: usunięte produkty `ProductID` wartości są wyświetlane poniżej widoku GridView ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](batch-deleting-cs/_static/image10.png))

> [!NOTE]
> Aby przetestować niepodzielną metodę `DeleteProductsWithTransaction`, ręcznie Dodaj wpis dla produktu w tabeli `Order Details`, a następnie spróbuj usunąć ten produkt (razem z innymi). Podczas próby usunięcia produktu ze skojarzoną kolejnością zostanie wyświetlone naruszenie ograniczenia klucza obcego, ale należy zwrócić uwagę na to, jak zostaną wycofane inne wybrane produkty.

## <a name="summary"></a>Podsumowanie

Tworzenie wsadowego usuwania interfejsu obejmuje dodanie widoku GridView z kolumną pól wyboru i kontrolki sieci Web przycisku, która po kliknięciu spowoduje usunięcie wszystkich zaznaczonych wierszy jako pojedynczej operacji niepodzielnej. W tym samouczku utworzyliśmy takie interfejsy przez zszywania razem pracy wykonanej w dwóch wcześniejszych samouczkach, [dodając kolumnę w widoku GridView](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs.md) i wprowadzając [modyfikacje bazy danych w ramach transakcji](wrapping-database-modifications-within-a-transaction-cs.md). W pierwszym samouczku utworzyliśmy widok GridView z kolumną pól wyboru, a w tym ostatnim zaimplementowano metodę w LOGIKI biznesowej, która w przypadku przekazaną `List<T>` wartości `ProductID`, usunięto je wszystkie w zakresie transakcji.

W następnym samouczku utworzysz interfejs do wykonywania operacji wstawiania wsadowego.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci liderzy dla tego samouczka to Hilton Giesenow i Teresa Murphy. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](batch-updating-cs.md)
> [dalej](batch-inserting-cs.md)

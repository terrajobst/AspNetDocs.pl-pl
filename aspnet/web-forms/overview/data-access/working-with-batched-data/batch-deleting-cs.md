---
uid: web-forms/overview/data-access/working-with-batched-data/batch-deleting-cs
title: Batch usuwania (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Dowiedz się, jak usunąć wiele rekordów bazy danych w ramach jednej operacji. W warstwie interfejsu użytkownika możemy bazują na rozszerzonego widoku GridView utworzone we wcześniejszej tut...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: ac6916d0-a5ab-4218-9760-7ba9e72d258c
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: c5b4d3c21fad9000ae50ecb35a5d94d176a135ee
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57069569"
---
<a name="batch-deleting-c"></a>Usuwanie w partiach (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_65_CS.zip) lub [Pobierz plik PDF](batch-deleting-cs/_static/datatutorial65cs1.pdf)

> Dowiedz się, jak usunąć wiele rekordów bazy danych w ramach jednej operacji. W warstwie interfejsu użytkownika możemy bazują na rozszerzonego widoku GridView utworzoną w samouczku wcześniej. W warstwie dostępu do danych Firma opakować wiele operacji usuwania w ramach transakcji, aby upewnić się, że wszystkie operacje usuwania powiedzie się lub wszystkie operacje usuwania są przywracane.


## <a name="introduction"></a>Wprowadzenie

[Poprzedni Samouczek](batch-updating-cs.md) zbadano sposób tworzenia partii edycji interfejs przy użyciu GridView pełni edytowalne. W sytuacjach, gdzie użytkownicy często edytowania wielu rekordów naraz, partii edytowanie interfejsu wymaga znacznie mniejszej liczby ogłaszania zwrotnego i mysz klawiatury kontekstu przełączników, a więc poprawa wydajności s użytkownika końcowego. Ta metoda przydaje się podobnie dla stron, gdzie jest typowe dla użytkowników usunąć wiele rekordów w jednym z rzeczywistym użyciem.

Każdy, kto został użyty w kliencie poczty e-mail w trybie online jest już znane z jednym z najbardziej typowych partii usuwanie interfejsów: pole wyboru w każdym wierszu w siatce z odpowiedniego Usuń wszystkie zaznaczone elementy przycisku (patrz rysunek 1). W tym samouczku jest raczej krótkie ponieważ możemy ve ustanowionego wszystkie trudną pracę w poprzednich samouczkach w tworzeniu interfejsu opartego na sieci web i metodę, aby usunąć serii rekordów w jednej niepodzielnej operacji. W [Dodawanie kolumny pól wyboru do kontrolki GridView](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs.md) samouczku utworzyliśmy GridView z kolumny pól wyboru, a w polu [opakowywanie modyfikacji bazy danych w ramach transakcji](wrapping-database-modifications-within-a-transaction-cs.md) samouczku utworzyliśmy metody w LOGIKI, która będzie używać transakcji można usunąć `List<T>` z `ProductID` wartości. W tym samouczku Zapoznamy bazują na i scalić naszych doświadczeń poprzedniej, aby utworzyć instancję pracy usuwania przykład.


[![Każdy wiersz zawiera pola wyboru](batch-deleting-cs/_static/image1.gif)](batch-deleting-cs/_static/image1.png)

**Rysunek 1**: Każdy wiersz zawiera pola wyboru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-deleting-cs/_static/image2.png))


## <a name="step-1-creating-the-batch-deleting-interface"></a>Krok 1. Tworzenie partii Usuwanie interfejsu

Ponieważ utworzyliśmy partii Usuwanie interfejsu w [Dodawanie kolumny pól wyboru do kontrolki GridView](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs.md) samouczek, możemy po prostu skopiować go do `BatchDelete.aspx` zamiast go utworzyć od podstaw. Zacznij od otwarcia `BatchDelete.aspx` strony w `BatchData` folder i `CheckBoxField.aspx` stronie `EnhancedGridView` folderu. Z `CheckBoxField.aspx` strony, przejdź do widoku źródła i skopiuj znaczników między `<asp:Content>` tagów, jak pokazano na rysunku 2.


[![Skopiuj oznaczeniu deklaracyjnym CheckBoxField.aspx do Schowka](batch-deleting-cs/_static/image2.gif)](batch-deleting-cs/_static/image3.png)

**Rysunek 2**: Skopiuj oznaczeniu deklaracyjnym `CheckBoxField.aspx` do Schowka ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-deleting-cs/_static/image4.png))


Następnie przejdź do widoku źródła `BatchDelete.aspx` i Wklej zawartość Schowka w ramach `<asp:Content>` tagów. Skopiować i wkleić kod z w obrębie klasy związane z kodem w `CheckBoxField.aspx.cs` do w klasie CodeBehind w `BatchDelete.aspx.cs` ( `DeleteSelectedProducts` przycisk s `Click` programu obsługi zdarzeń `ToggleCheckState` metody i `Click` procedury obsługi zdarzeń Aby uzyskać `CheckAll` i `UncheckAll` przyciski). Po skopiowaniu za pośrednictwem tej zawartości, `BatchDelete.aspx` klasy CodeBehind strony s powinna zawierać następujący kod:


[!code-csharp[Main](batch-deleting-cs/samples/sample1.cs)]

Po skopiowaniu w oznaczeniu deklaracyjnym i kod źródłowy, Poświęć chwilę na testowanie `BatchDelete.aspx` , wyświetlając go za pośrednictwem przeglądarki. Powinien zostać wyświetlony GridView zawierającą listę pierwszych dziesięciu produktów w GridView, przy czym każdy wiersz ofercie nazwa produktu s, kategorii i cenę wraz z pola wyboru. Powinny być trzy przyciski: Zaznacz wszystko, usuń zaznaczenie wszystkich i Usuń wybranych produktów. Kliknięcie przycisku Sprawdź wszystkie wybiera wszystkie pola wyboru, podczas gdy Usuń zaznaczenie wszystkich czyści wszystkie pola wyboru. Klikając polecenie Usuń wybrane produkty wyświetla komunikat, który zawiera listę `ProductID` wartości wybranych produktów, ale faktycznie nie usuwa produktów.


[![Interfejs z CheckBoxField.aspx została przeniesiona do BatchDeleting.aspx](batch-deleting-cs/_static/image3.gif)](batch-deleting-cs/_static/image5.png)

**Rysunek 3**: Interfejs z `CheckBoxField.aspx` została przeniesiona do `BatchDeleting.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-deleting-cs/_static/image6.png))


## <a name="step-2-deleting-the-checked-products-using-transactions"></a>Krok 2. Usunięcie zaznaczenia produktów, za pomocą transakcji

Za pomocą usługi batch, usuwanie pomyślnie kopiowane do interfejsu `BatchDeleting.aspx`, wszystkie, że pozostaje tylko zaktualizować kod, tak aby przycisk Usuń wybrane produkty Usuwa zaznaczony produkty, za pomocą `DeleteProductsWithTransaction` method in Class metoda `ProductsBLL` klasy. Ta metoda dodane w [opakowywanie modyfikacji bazy danych w ramach transakcji](wrapping-database-modifications-within-a-transaction-cs.md) samouczek, przyjmuje jako dane wejściowe `List<T>` z `ProductID` wartości i usuwa poszczególnych `ProductID` w zakresie transakcja.

`DeleteSelectedProducts` Przycisk s `Click` programu obsługi zdarzeń aktualnie używa następujących `foreach` pętli do iteracji przez każdego wiersza w widoku GridView:


[!code-csharp[Main](batch-deleting-cs/samples/sample2.cs)]

Dla każdego wiersza `ProductSelector` programowo odwołuje się do kontrolki pola wyboru w sieci Web. Jeśli jest ono zaznaczone, wiersz s `ProductID` jest pobierana z `DataKeys` kolekcji i `DeleteResults` etykiety s `Text` właściwość zostanie zaktualizowany i będzie zawierać komunikat wskazujący, że wiersz został wybrany do usunięcia.

Powyższy kod nie usuwa wszystkie rekordy co wywołanie `ProductsBLL` klasy s `Delete` metody jest opatrzona komentarzem. Były tę logikę usuwania mają być stosowane, kod może usunąć produktów, ale nie w obrębie operacją niepodzielną. Oznacza to jeśli pierwszy usuwa kilka w sekwencji zakończyło się pomyślnie, ale późniejszy go nie powiodło się (np. z powodu naruszenia ograniczenia klucza obcego), czy zgłoszony wyjątek, ale tych produktów, które już usunięte pozostają usunięte.

Aby zapewnić niepodzielność, należy zamiast tego użyć `ProductsBLL` klasy s `DeleteProductsWithTransaction` metody. Ponieważ ta metoda przyjmuje listę `ProductID` wartości, należy najpierw skompilować tę listę z siatki, a następnie przekazać go jako parametr. Należy najpierw utworzyć wystąpienie `List<T>` typu `int`. W ramach `foreach` pętli, musimy dodać wybrane produkty `ProductID` wartości tej `List<T>`. Po pętli to `List<T>` muszą zostać przekazane do `ProductsBLL` klasy s `DeleteProductsWithTransaction` metody. Aktualizacja `DeleteSelectedProducts` przycisk s `Click` program obsługi zdarzeń z następującym kodem:


[!code-csharp[Main](batch-deleting-cs/samples/sample3.cs)]

Zaktualizowano kod tworzy `List<T>` typu `int` (`productIDsToDelete`) i wypełnia ją za pomocą `ProductID` wartości do usunięcia. Po `foreach` pętli, jeśli istnieje co najmniej jeden produkt z wybraniu `ProductsBLL` klasy s `DeleteProductsWithTransaction` metoda jest nazywana i przekazywana tej listy. `DeleteResults` Także jest wyświetlana etykieta i dane odbitych do kontrolki GridView (tak, aby nowo usunięte rekordy są już wyświetlane jako wiersze w siatce).

Rysunek 4 przedstawia widoku GridView po liczbę wierszy zostały wybrane do usunięcia. Rysunek 5. pokazuje ekranu natychmiast, po kliknięciu przycisku Usuń wybrane produkty. Należy pamiętać, że na rysunku 5 `ProductID` wartości usunięte rekordy są wyświetlane w etykiecie poniżej kontrolki GridView i tych wierszy nie są już w widoku GridView.


[![Wybrane produkty zostaną usunięte.](batch-deleting-cs/_static/image4.gif)](batch-deleting-cs/_static/image7.png)

**Rysunek 4**: Wybrane produkty zostaną usunięte ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-deleting-cs/_static/image8.png))


[![Wartości usunięte ProductID produkty są wymienione poniżej GridView](batch-deleting-cs/_static/image5.gif)](batch-deleting-cs/_static/image9.png)

**Rysunek 5**: Usunięto produktów `ProductID` wartości są wymienione poniżej kontrolki GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-deleting-cs/_static/image10.png))


> [!NOTE]
> Aby przetestować `DeleteProductsWithTransaction` niepodzielność metoda s, ręcznie dodaj wpis dla produktu w `Order Details` tabeli, a następnie spróbuj usunąć produktu (wraz z innymi). Naruszenie ograniczenia klucza obcego zostanie wyświetlony podczas próby usunąć produkt przy użyciu skojarzonego zamówienia, ale należy pamiętać, jak inne usunięcia wybrane produkty zostaną wycofane.


## <a name="summary"></a>Podsumowanie

Tworzenie partii Usuwanie interfejsu obejmuje dodawanie kolumny pól wyboru z GridView i kontrolkę przycisku w sieci Web, która, po kliknięciu, Usuń wszystkie zaznaczone wiersze jako jednej niepodzielnej operacji. W tym samouczku utworzyliśmy taki interfejs przez zszywania pracy wykonanej w dwóch poprzednich samouczkach [Dodawanie kolumny pól wyboru do kontrolki GridView](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs.md) i [opakowywanie modyfikacji bazy danych w ramach transakcji](wrapping-database-modifications-within-a-transaction-cs.md). W pierwszym samouczku utworzyliśmy GridView z kolumny pól wyboru, a w drugim wdrożyliśmy metody w LOGIKI, gdy przekazywane `List<T>` z `ProductID` wartości, usunąć je wszystkie w zakresie transakcji.

W następnym samouczku utworzymy interfejs służący do wykonywania operacji wstawiania wsadowego.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Wiodące osób dokonujących przeglądu, w tym samouczku zostały Hilton Giesenow i Teresa Murphy. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](batch-updating-cs.md)
> [dalej](batch-inserting-cs.md)

---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs
title: Dodawanie kolumny w widoku GridView (C#) | Microsoft Docs
author: rick-anderson
description: W tym samouczku pokazano, jak dodać kolumnę pól wyboru do kontrolki GridView, aby zapewnić użytkownikowi intuicyjny sposób wybierania wielu wierszy w G...
ms.author: riande
ms.date: 03/06/2007
ms.assetid: f63a9443-2db0-4f80-8246-840d3e86c2a3
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: b9d1ed50e8d88202c3286b4cd0e9ebf111dfbe21
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74592698"
---
# <a name="adding-a-gridview-column-of-checkboxes-c"></a>Dodawanie kolumny pól wyboru do kontrolki GridView (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_52_CS.exe) lub [Pobierz plik PDF](adding-a-gridview-column-of-checkboxes-cs/_static/datatutorial52cs1.pdf)

> W tym samouczku pokazano, jak dodać kolumnę pól wyboru do kontrolki GridView, aby zapewnić użytkownikowi intuicyjny sposób wybierania wielu wierszy widoku GridView.

## <a name="introduction"></a>Wprowadzenie

W poprzednim samouczku sprawdziłem, jak dodać kolumnę przycisków radiowych do widoku GridView na potrzeby wyboru określonego rekordu. Kolumna przycisków radiowych jest odpowiednim interfejsem użytkownika, gdy użytkownik jest ograniczony do wybierania maksymalnie jednego elementu z siatki. Czasami jednak może chcieć zezwolić użytkownikowi na wybranie dowolnej liczby elementów z siatki. Klienci poczty e-mail opartej na sieci Web, na przykład, zazwyczaj wyświetlają listę komunikatów z kolumną pól wyboru. Użytkownik może wybrać dowolną liczbę komunikatów, a następnie wykonać pewne czynności, takie jak przeniesienie wiadomości e-mail do innego folderu lub usunięcie.

W tym samouczku dowiesz się, jak dodać kolumnę pól wyboru i jak określić, które pola wyboru zostały sprawdzone na serwerze ogłaszania zwrotnego. W szczególności utworzymy przykład, który ściśle naśladuje interfejs użytkownika klienta poczty e-mail w sieci Web. Nasz przykład zawiera widok z stronicowaną stroną zawierającą produkty w `Products` tabeli bazy danych z CheckBox w każdym wierszu (patrz rysunek 1). Przycisk Usuń wybrane produkty, gdy zostanie kliknięty, spowoduje usunięcie wybranych produktów.

[![każdy wiersz produktu zawiera pole wyboru](adding-a-gridview-column-of-checkboxes-cs/_static/image1.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image1.png)

**Rysunek 1**. Każdy wiersz produktu zawiera pole wyboru ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-a-gridview-column-of-checkboxes-cs/_static/image2.png))

## <a name="step-1-adding-a-paged-gridview-that-lists-product-information"></a>Krok 1. Dodawanie stronicowanego elementu GridView zawierającego listę informacji o produkcie

Przed zajęciem dalszych informacji o dodawaniu kolumny pól wyboru poczekaj, aż zamienisz produkty w GridView, który obsługuje stronicowanie. Zacznij od otwarcia strony `CheckBoxField.aspx` w folderze `EnhancedGridView` i przeciągnij widok GridView z przybornika do projektanta, ustawiając jego `ID` na `Products`. Następnie wybierz opcję powiązania widoku GridView z nowym elementem ObjectDataSource o nazwie `ProductsDataSource`. Skonfiguruj element ObjectDataSource, aby używał klasy `ProductsBLL`, wywołując metodę `GetProducts()` w celu zwrócenia danych. Ponieważ ten element GridView będzie tylko do odczytu, Ustaw listę rozwijaną na kartach UPDATE, INSERT i DELETE na wartość (brak).

[![utworzyć nowego elementu ObjectDataSource o nazwie ProductsDataSource](adding-a-gridview-column-of-checkboxes-cs/_static/image2.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image3.png)

**Rysunek 2**. Utwórz nowy element ObjectDataSource o nazwie `ProductsDataSource` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-a-gridview-column-of-checkboxes-cs/_static/image4.png))

[![skonfigurować element ObjectDataSource do pobierania danych za pomocą metody getproductss ()](adding-a-gridview-column-of-checkboxes-cs/_static/image3.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image5.png)

**Rysunek 3**. Konfigurowanie elementu ObjectDataSource do pobierania danych przy użyciu metody `GetProducts()` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-a-gridview-column-of-checkboxes-cs/_static/image6.png))

[![ustawić listy rozwijane na kartach Aktualizuj, Wstaw i Usuń do (brak).](adding-a-gridview-column-of-checkboxes-cs/_static/image4.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image7.png)

**Rysunek 4**. Ustawianie list rozwijanych w kartach Aktualizuj, Wstaw i Usuń do (brak) ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-a-gridview-column-of-checkboxes-cs/_static/image8.png))

Po zakończeniu działania Kreatora konfiguracji źródła danych program Visual Studio automatycznie utworzy elementy BoundColumn i CheckBoxColumn dla pól danych związanych z produktem. Podobnie jak w poprzednim samouczku, Usuń wszystkie oprócz `ProductName`, `CategoryName`i `UnitPrice` BoundFields, a następnie zmień właściwości `HeaderText` na Product, Category i Price. Skonfiguruj `UnitPrice` BoundField tak, aby jego wartość była sformatowana jako waluta. Ponadto skonfiguruj widok GridView, aby obsługiwał stronicowanie, zaznaczając pole wyboru Włącz stronicowanie z tagu inteligentnego.

Zezwól również na dodanie interfejsu użytkownika do usuwania wybranych produktów. Dodaj kontrolkę sieci Web przycisku poniżej widoku GridView, ustawiając jej `ID` na `DeleteSelectedProducts` i jej Właściwość `Text`, aby usunąć wybrane produkty. Zamiast usuwać produkty z bazy danych, w tym przykładzie po prostu zostanie wyświetlony komunikat z informacją o produktach, które zostałyby usunięte. Aby to umożliwić, Dodaj kontrolkę sieci Web etykieta pod przyciskiem. Ustaw jej identyfikator na `DeleteResults`, wyczyść Właściwość `Text` i ustaw jej właściwości `Visible` i `EnableViewState` na `false`.

Po wprowadzeniu tych zmian znaczniki deklaratywne, ObjectDataSource, Button i Label s powinny wyglądać podobnie do następujących:

[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample1.aspx)]

Poświęć chwilę na wyświetlenie strony w przeglądarce (patrz rysunek 5). W tym momencie powinna zostać wyświetlona nazwa, Kategoria i cena pierwszych dziesięciu produktów.

[![nazwa, Kategoria i cena pierwszych dziesięciu produktów znajduje się na liście](adding-a-gridview-column-of-checkboxes-cs/_static/image5.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image9.png)

**Rysunek 5**: nazwa, Kategoria i cena pierwszych dziesięciu produktów są wymienione na liście ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-checkboxes-cs/_static/image10.png))

## <a name="step-2-adding-a-column-of-checkboxes"></a>Krok 2. Dodawanie kolumny pól wyboru

Ponieważ ASP.NET 2,0 zawiera CheckBoxField, może to oznaczać, że można go użyć do dodania kolumny pól wyboru do widoku GridView. Niestety, nie jest to przypadek, ponieważ CheckBoxField jest przeznaczony do pracy z polem danych wartości logicznych. Oznacza to, że w celu użycia CheckBoxField musimy określić bazowe pole danych, którego wartość jest konsultowana, aby określić, czy renderowane pole wyboru jest zaznaczone. Nie można użyć CheckBoxField do dołączenia kolumny niezaznaczonych pól wyboru.

Zamiast tego należy dodać TemplateField i dodać kontrolkę sieci Web CheckBox do `ItemTemplate`. Przejdź dalej i Dodaj TemplateField do `Products` GridView i wprowadź pierwsze (daleko do lewej) pole. W tagu inteligentnym GridView kliknij link Edytuj szablony, a następnie przeciągnij formant sieci Web CheckBox z przybornika do `ItemTemplate`. Ustaw tę właściwość `ID` CheckBox na `ProductSelector`.

[![dodać kontrolkę sieci Web CheckBox o nazwie ProductSelector do TemplateField s ItemTemplate](adding-a-gridview-column-of-checkboxes-cs/_static/image6.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image11.png)

**Ilustracja 6**. Dodawanie kontrolki sieci Web CheckBox o nazwie `ProductSelector` do `ItemTemplate` TemplateField ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-a-gridview-column-of-checkboxes-cs/_static/image12.png))

Po dodaniu kontrolki sieci Web TemplateField i CheckBox każdy wiersz zawiera teraz pole wyboru. Rysunek 7 przedstawia Tę stronę, gdy jest wyświetlany za pomocą przeglądarki, po dodaniu pola wyboru TemplateField i.

[![każdy wiersz produktu zawiera teraz pole wyboru](adding-a-gridview-column-of-checkboxes-cs/_static/image7.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image13.png)

**Rysunek 7**: każdy wiersz produktu zawiera teraz pole wyboru ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-a-gridview-column-of-checkboxes-cs/_static/image14.png))

## <a name="step-3-determining-what-checkboxes-were-checked-on-postback"></a>Krok 3. Określanie, które pola wyboru zostały sprawdzone na ogłaszaniu zwrotnym

W tym momencie mamy kolumnę pól wyboru, ale nie chcesz określać, które pola wyboru zostały sprawdzone w odniesieniu do ogłaszania zwrotnego. Po kliknięciu przycisku Usuń wybrane produkty należy wiedzieć, jakie pola wyboru zostały zaznaczone, aby usunąć te produkty.

Właściwość GridView s [`Rows`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows.aspx) zapewnia dostęp do wierszy danych w widoku GridView. Możemy wykonać iterację tych wierszy, programowo uzyskać dostęp do kontrolki CheckBox, a następnie zajrzeć do jej właściwości `Checked`, aby określić, czy pole wyboru zostało zaznaczone.

Utwórz procedurę obsługi zdarzeń dla `DeleteSelectedProducts` przycisku `Click` kontrolki sieci Web, a następnie Dodaj następujący kod:

[!code-csharp[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample2.cs)]

Właściwość `Rows` zwraca kolekcję wystąpień `GridViewRow`, które korzeń wiersze danych GridView. W tym miejscu `foreach` pętla wylicza tę kolekcję. Dla każdego obiektu `GridViewRow` pole wyboru wiersza s jest dostępne programowo przy użyciu `row.FindControl("controlID")`. Jeśli pole wyboru jest zaznaczone, wiersz s odpowiadający wartości `ProductID` jest pobierany z kolekcji `DataKeys`. W tym ćwiczeniu po prostu wyświetlamy komunikat informacyjny w etykiecie `DeleteResults`, chociaż w działającej aplikacji My d zamiast tego nastąpi wywołanie metody `DeleteProduct(productID)` `ProductsBLL` klasy s.

Po dodaniu tego programu obsługi zdarzeń kliknięcie przycisku Usuń wybrane produkty spowoduje wyświetlenie `ProductID` s wybranych produktów.

[![po kliknięciu przycisku Usuń wybrane produkty zostaną wyświetlone wybrane produkty ProductID](adding-a-gridview-column-of-checkboxes-cs/_static/image8.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image15.png)

**Ilustracja 8**. po kliknięciu przycisku Usuń wybrane produkty na liście zostaną wyświetlone wybrane produkty `ProductID` s ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-a-gridview-column-of-checkboxes-cs/_static/image16.png))

## <a name="step-4-adding-check-all-and-uncheck-all-buttons"></a>Krok 4. dodanie opcji Zaznacz wszystko i usuń zaznaczenie wszystkich przycisków

Jeśli użytkownik chce usunąć wszystkie produkty na bieżącej stronie, musi zaznaczyć każdy z dziesięciu pól wyboru. Możemy pomóc przyspieszyć ten proces, dodając przycisk Zaznacz wszystko, który po kliknięciu zaznacza wszystkie pola wyboru w siatce. Przycisk Usuń wszystko jest równie przydatny.

Dodaj dwa kontrolki sieci Web Button do strony, umieszczając je nad GridView. Ustaw `ID` pierwszej `CheckAll` i Właściwość `Text` do sprawdzenia wszystkich; Ustaw drugą `ID` s `UncheckAll` i jej Właściwość `Text`, aby usunąć zaznaczenie wszystkiego.

[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample3.aspx)]

Następnie Utwórz metodę w klasie związanej z kodem o nazwie `ToggleCheckState(checkState)`, która po wywołaniu powoduje Wyliczenie `Products` widoku GridView `Rows` i ustawia każdą właściwość CheckBox s `Checked` na wartość przekazaną w parametrze *wartość CheckState zaznaczonego* .

[!code-csharp[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample4.cs)]

Następnie utwórz obsługę zdarzeń `Click` dla przycisków `CheckAll` i `UncheckAll`. W programie obsługi zdarzeń `CheckAll` s po prostu wywołaj `ToggleCheckState(true)`; w `UncheckAll`Wywołaj `ToggleCheckState(false)`.

[!code-csharp[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample5.cs)]

Po wybraniu tego kodu po kliknięciu przycisku Zaznacz wszystko powoduje ogłoszenie zwrotne i sprawdza wszystkie pola wyboru w widoku GridView. Analogicznie, kliknięcie pozycji Usuń wszystkie powoduje usunięcie zaznaczenia wszystkich pól wyboru. Rysunek 9 przedstawia ekran po sprawdzeniu, czy przycisk Sprawdź wszystko zostało zaznaczone.

[![kliknięcie przycisku Zaznacz wszystko powoduje zaznaczenie wszystkich pól wyboru](adding-a-gridview-column-of-checkboxes-cs/_static/image9.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image17.png)

**Ilustracja 9**. kliknięcie przycisku Zaznacz wszystko powoduje zaznaczenie wszystkich pól wyboru ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-a-gridview-column-of-checkboxes-cs/_static/image18.png))

> [!NOTE]
> Podczas wyświetlania kolumny pól wyboru jedno z metod zaznaczania lub zaznaczania zaznaczenia wszystkich pól wyboru jest za pomocą pola wyboru w wierszu nagłówka. Ponadto bieżąca klauzula Check All/Usuń zaznaczenie wszystkich implementacji wymaga odświeżenia. Pola wyboru można sprawdzić lub zaznaczyć, całkowicie za pomocą skryptu po stronie klienta, zapewniając tym samym środowisko użytkownika snappier. Aby eksplorować przy użyciu pola wyboru wiersza nagłówka dla opcji Zaznacz wszystko i odznacz wszystkie szczegóły, a także dyskusje na temat korzystania z technik po stronie klienta, sprawdź [zaznaczenie wszystkich pól wyboru w widoku GridView przy użyciu skryptu po stronie klienta i pola wyboru Zaznacz wszystko](http://aspnet.4guysfromrolla.com/articles/053106-1.aspx).

## <a name="summary"></a>Podsumowanie

W przypadkach, gdy musisz zezwolić użytkownikom na wybranie dowolnej liczby wierszy z widoku GridView przed kontynuowaniem, Dodawanie kolumny pól wyboru jest jedną z opcji. Jak zostało to opisane w tym samouczku, wraz z kolumną pól wyboru w widoku GridView pociąga za sobą dodanie TemplateField z kontrolką sieci Web CheckBox. Za pomocą kontrolki sieci Web (w przeciwieństwie do dodawania znaczników bezpośrednio do szablonu, jak w poprzednim samouczku) ASP.NET automatycznie zapamiętuje pola wyboru i nie zostały zaewidencjonowane przez ogłaszanie zwrotne. Możemy również programowo uzyskać dostęp do pól wyboru w kodzie, aby określić, czy dane pole wyboru jest zaznaczone, lub zmienić stan zaznaczenia.

W tym samouczku i ostatnim z nich Oglądano Dodawanie kolumny selektora wierszy do widoku GridView. W naszym następnym samouczku sprawdzimy, w jaki sposób, z chwilą pracy, możemy dodać możliwości wstawiania do widoku GridView.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Poprzednie](adding-a-gridview-column-of-radio-buttons-cs.md)
> [dalej](inserting-a-new-record-from-the-gridview-s-footer-cs.md)

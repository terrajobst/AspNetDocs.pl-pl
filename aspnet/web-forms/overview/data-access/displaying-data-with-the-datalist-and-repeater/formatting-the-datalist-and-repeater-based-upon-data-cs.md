---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs
title: Formatowanie elementu DataList i wzmacniak na podstawie danych (C#) | Microsoft Docs
author: rick-anderson
description: W tym samouczku poznasz Przykłady sposobu formatowania wyglądu kontrolek DataList i wzmacniak przy użyciu funkcji formatowania z...
ms.author: riande
ms.date: 09/13/2006
ms.assetid: 83e3d759-82b8-41e6-8d62-f0f4b3edec41
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 68de3450ed97fc7bd0efb27e089d9db8e3e85fb0
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74633228"
---
# <a name="formatting-the-datalist-and-repeater-based-upon-data-c"></a>Formatowanie elementów DataList i Repeater na podstawie danych (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_30_CS.exe) lub [Pobierz plik PDF](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/datatutorial30cs1.pdf)

> W tym samouczku przejdziemy przez Przykłady sposobu formatowania wyglądu kontrolek DataList i wzmacniak przy użyciu funkcji formatowania w szablonach lub przez obsługę zdarzenia powiązanego z danymi.

## <a name="introduction"></a>Wprowadzenie

Jak zostało to opisane w poprzednim samouczku, element DataList oferuje wiele właściwości związanych z stylem, które wpływają na jego wygląd. W szczególności przedstawiono sposób przypisywania domyślnych klas CSS do właściwości DataList s `HeaderStyle`, `ItemStyle`, `AlternatingItemStyle`i `SelectedItemStyle`. Oprócz tych czterech właściwości, element DataList zawiera wiele innych właściwości związanych z stylem, takich jak `Font`, `ForeColor`, `BackColor`i `BorderWidth`, aby nazwać kilka. Formant wzmacniania nie zawiera żadnych właściwości związanych z stylem. Wszystkie takie ustawienia stylu muszą być wprowadzane bezpośrednio w znacznikach w szablonach wzmacniania.

Często jednak, jak dane powinny być formatowane, zależy od samych danych. Na przykład podczas tworzenia listy produktów firma Microsoft może chcieć wyświetlić informacje o produkcie w kolorze szarej czcionki, jeśli jest on nieaktywny lub chcemy wyróżnić wartość `UnitsInStock`, jeśli jest równa zero. Jak pokazano w poprzednich samouczkach, widok GridView, DetailsView i FormView oferuje dwa różne sposoby formatowania ich wyglądu w oparciu o dane:

- **Zdarzenie `DataBound`** Utwórz procedurę obsługi zdarzeń dla odpowiedniego zdarzenia `DataBound`, które zostanie wyzwolone po powiązaniu danych z każdym elementem (dla widoku GridView była to zdarzenie `RowDataBound`; dla elementu DataList i wzmacniak jest to zdarzenie `ItemDataBound`). W tym obsłudze zdarzeń można zbadać, że dane są powiązane, a podejmowane decyzje dotyczące formatowania. Ta technika została sprawdzona w przypadku [formatowania niestandardowego na podstawie](../custom-formatting/custom-formatting-based-upon-data-cs.md) samouczka dotyczącego danych.
- Funkcja **formatowania w szablonach** w przypadku używania używanie TemplateField w kontrolkach DetailsView lub GridView, lub szablon w kontrolce FormView, możemy dodać funkcję formatowania do klasy "ASP.NET" powiązanej z kodem strony, warstwy logiki biznesowej lub innej biblioteki klas dostępnej z aplikacji sieci Web. Ta funkcja formatowania może akceptować dowolną liczbę parametrów wejściowych, ale musi zwrócić kod HTML, aby renderować w szablonie. Funkcja formatowania została najpierw sprawdzona w [Używanie TemplateField Using w samouczku kontrolki GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) .

Obie te techniki formatowania są dostępne z kontrolkami DataList i wzmacniak. W tym samouczku przejdziemy do przykładów przy użyciu obu technik dla obu formantów.

## <a name="using-theitemdataboundevent-handler"></a>Korzystanie z programu obsługi zdarzeń`ItemDataBound`

Gdy dane są powiązane z elementem DataList, albo z kontrolki źródła danych lub programowo przypisują dane do właściwości Control-`DataSource` i wywołując metodę `DataBind()`, wyzwalane jest zdarzenie DataList s `DataBinding`, zostało ono wyliczone, a każdy rekord danych jest powiązany z elementem DataList. Dla każdego rekordu w źródle danych, element DataList tworzy obiekt [`DataListItem`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.aspx) , który następnie jest powiązany z bieżącym rekordem. W trakcie tego procesu, ta DataList wywołuje dwa zdarzenia:

- **`ItemCreated`** uruchamiany po utworzeniu `DataListItem`
- **`ItemDataBound`** uruchamiany po powiązaniu bieżącego rekordu z `DataListItem`

Poniższe kroki przedstawiają proces powiązania danych dla formantu DataList.

1. Wyzwolono [zdarzenie`DataBinding`](https://msdn.microsoft.com/library/system.web.ui.control.databinding.aspx) DataList s
2. Dane są powiązane z elementem DataList  
  
   Dla każdego rekordu w źródle danych 

    1. Tworzenie obiektu `DataListItem`
    2. Wyzwalanie [zdarzenia`ItemCreated`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemcreated.aspx)
    3. Powiąż rekord z `DataListItem`
    4. Wyzwalanie [zdarzenia`ItemDataBound`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemdatabound.aspx)
    5. Dodawanie `DataListItem` do kolekcji `Items`

Po powiązaniu danych z formantem wzmacniania Postęp przechodzi przez dokładnie tę samą sekwencję kroków. Jedyną różnicą jest to, że zamiast tworzonych wystąpień `DataListItem` wzmacniak używa [`RepeaterItem`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem(VS.80).aspx)s.

> [!NOTE]
> Czytnik uważni mógł zauważyć niewielką anomalię między sekwencją kroków, które wykonywanych, gdy element DataList i wzmacniak są powiązane z danymi, a gdy GridView jest powiązany z danymi. Na końcu procesu powiązania danych element GridView wywołuje zdarzenie `DataBound`; Jednakże ani formant DataList ani wzmacniak nie mają takiego zdarzenia. Wynika to z faktu, że kontrolki DataList i wzmacniak zostały utworzone z powrotem w ASP.NET 1. x.

Podobnie jak w przypadku widoku GridView, jedną z opcji formatowania na podstawie danych jest utworzenie programu obsługi zdarzeń dla zdarzenia `ItemDataBound`. Ta procedura obsługi zdarzeń sprawdzi dane, które zostały właśnie powiązane z `DataListItem` lub `RepeaterItem` i wpływają na formatowanie kontrolki zgodnie z wymaganiami.

W przypadku formantu DataList zmiany formatowania dla całego elementu można zaimplementować przy użyciu właściwości `DataListItem` s, które obejmują `Font`standardowe, `ForeColor`, `BackColor`, `CssClass`itd. Aby mieć wpływ na formatowanie określonych kontrolek sieci Web w ramach szablonu DataList s, musimy programowo uzyskać dostęp do stylu tych formantów sieci Web i zmodyfikować go. Znaleźliśmy, jak to zrobić z powrotem w *niestandardowym formatowaniu opartym* na samouczku danych. Podobnie jak w przypadku kontrolki wzmacniak, Klasa `RepeaterItem` nie ma właściwości związanych z stylem; w związku z tym wszystkie zmiany dotyczące stylu wprowadzone do `RepeaterItem` w programie obsługi zdarzeń `ItemDataBound` muszą zostać wykonane przez programowe uzyskiwanie dostępu do formantów sieci Web w ramach szablonu i aktualizowanie ich.

Ponieważ technika formatowania `ItemDataBound` dla elementu DataList i wzmacniak jest niemal identyczna, nasz przykład zakoncentruje się na użyciu elementu DataList.

## <a name="step-1-displaying-product-information-in-the-datalist"></a>Krok 1. Wyświetlanie informacji o produkcie w elemencie DataList

Przed zajmować się formatowaniem należy najpierw utworzyć stronę, która używa elementu DataList do wyświetlania informacji o produkcie. W [poprzednim samouczku](displaying-data-with-the-datalist-and-repeater-controls-cs.md) utworzyliśmy element DataList, którego `ItemTemplate` wyświetlać każdą nazwę produktu, kategorię, dostawcę, ilość na jednostkę i cenę. Pozwól, aby w tym samouczku powtórzyć tę funkcję. Aby to osiągnąć, można utworzyć ponownie element DataList i jego element ObjectDataSource od podstaw albo skopiować te kontrolki ze strony utworzonej w poprzednim samouczku (`Basics.aspx`) i wkleić je do strony dla tego samouczka (`Formatting.aspx`).

Po przeprowadzeniu replikacji funkcji DataList i ObjectDataSource z `Basics.aspx` do `Formatting.aspx`Poświęć chwilę na zmianę właściwości `ID` DataList s z `DataList1` na bardziej opisowy `ItemDataBoundFormattingExample`. Następnie Wyświetl element DataList w przeglądarce. Jak pokazano na rysunku 1, jedyną różnicą formatowania między każdym produktem jest to, że kolor tła jest alternatywny.

[![produkty są wymienione w kontrolce DataList](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image2.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image1.png)

**Rysunek 1**. produkty są wymienione w kontrolce DataList ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image3.png))

W tym samouczku pomożemy sformatować element DataList w taki sposób, że każdy produkt o cenie mniejszej niż $20,00 będzie miał wartość Nazwa i cena jednostkowa wyróżnione kolorem żółtym.

## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-itemdatabound-event-handler"></a>Krok 2. programistyczne Określanie wartości danych w programie obsługi zdarzeń ItemDataBound

Ze względu na to, że tylko produkty z ceną poniżej $20,00 będą miały zastosowane niestandardowe formatowanie, musi być w stanie określić cenę produktu s. Podczas wiązania danych z elementem DataList element DataList wylicza rekordy w źródle danych, a dla każdego rekordu tworzy wystąpienie `DataListItem`, wiążąc rekord źródła danych z `DataListItem`. Gdy dane określonego rekordu s zostaną powiązane z bieżącym obiektem `DataListItem`, zostanie wyzwolone zdarzenie "`ItemDataBound`a DataList s". Możemy utworzyć procedurę obsługi zdarzeń dla tego zdarzenia, aby sprawdzić wartości danych dla bieżącego `DataListItem` i, na podstawie tych wartości, wprowadzić wszelkie zmiany formatowania.

Utwórz zdarzenie `ItemDataBound` dla elementu DataList i Dodaj następujący kod:

[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample1.cs)]

Chociaż koncepcja i semantyka za pomocą procedury obsługi zdarzeń `ItemDataBound` DataList są takie same jak te, które są używane przez program obsługi zdarzeń GridView s `RowDataBound` w *niestandardowym formatowaniu opartym* na samouczku danych, Składnia różni się nieznacznie. Po wyjściu zdarzenia `ItemDataBound`, `DataListItem` jedynie powiązane z danymi są przesyłane do odpowiedniej procedury obsługi zdarzeń za pośrednictwem `e.Item` (zamiast `e.Row`, podobnie jak w przypadku programu obsługi zdarzeń `RowDataBound` GridView s). Procedura obsługi zdarzeń `ItemDataBound` DataList s jest wyzwalana dla *każdego* wiersza dodanego do elementu DataList, w tym wierszy nagłówka, wierszy stopki i wierszy separatora. Informacje o produkcie są jednak powiązane tylko z wierszami danych. W związku z tym, w przypadku użycia zdarzenia `ItemDataBound` do sprawdzenia danych powiązanych z elementem DataList, musimy najpierw upewnić się, że pracujemy nad danymi. Można to osiągnąć, sprawdzając [właściwość`ItemType`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.itemtype.aspx)`DataListItem` s, która może mieć jedną z [następujących ośmiu wartości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listitemtype.aspx):

- `AlternatingItem`
- `EditItem`
- `Footer`
- `Header`
- `Item`
- `Pager`
- `SelectedItem`
- `Separator`

Oba `Item` i `AlternatingItem``DataListItem` s korzeń elementy danych DataList s. Przy założeniu, że pracujemy z `Item` lub `AlternatingItem`, uzyskujemy dostęp do rzeczywistego wystąpienia `ProductsRow` powiązanego z bieżącym `DataListItem`. [Właściwość`DataItem`](https://msdn.microsoft.com/system.web.ui.webcontrols.datalistitem.dataitem.aspx) `DataListItem` s zawiera odwołanie do obiektu `DataRowView`, którego właściwość `Row` zawiera odwołanie do rzeczywistego obiektu `ProductsRow`.

Następnie sprawdzimy Właściwość `ProductsRow` wystąpienia s `UnitPrice`. Ponieważ pole "`UnitPrice` Products" umożliwia `NULL` wartości, przed podjęciem próby uzyskania dostępu do właściwości `UnitPrice` należy najpierw sprawdzić, czy ma ona `NULL` wartość przy użyciu metody `IsUnitPriceNull()`. Jeśli wartość `UnitPrice` nie jest `NULL`, Sprawdzamy, czy nie są one mniejsze niż $20,00. Jeśli wartość jest równa $20,00, należy zastosować niestandardowe formatowanie.

## <a name="step-3-highlighting-the-product-s-name-and-price"></a>Krok 3. wyróżnianie nazwy produktu i ceny

Gdy będziemy wiedzieć, że cena produktu s jest mniejsza niż $20,00, wszystko to jest w celu wyróżnienia jego nazwy i ceny. Aby to osiągnąć, należy najpierw programowo odwoływać się do kontrolek etykiet w `ItemTemplate`, w których wyświetlana jest nazwa i cena produktu. Następnie musimy wyświetlić żółte tło. Informacje o formatowaniu można zastosować bezpośrednio modyfikując etykiety `BackColor` właściwości (`LabelID.BackColor = Color.Yellow`); najlepiej, chociaż wszystkie kwestie dotyczące wyświetlania powinny być wyrażone za pośrednictwem kaskadowych arkuszy stylów. W rzeczywistości mamy już arkusz stylów, który udostępnia wymagane formatowanie zdefiniowane w `Styles.css` - `AffordablePriceEmphasis`, który został utworzony i omówiony w sekcji *formatowanie niestandardowe na podstawie* samouczka dotyczącego danych.

Aby zastosować formatowanie, po prostu ustaw dwie kontrolki sieci Web etykiet `CssClass` właściwości `AffordablePriceEmphasis`, jak pokazano w poniższym kodzie:

[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample2.cs)]

Po ukończeniu obsługi zdarzeń `ItemDataBound` ponownie odwiedź stronę `Formatting.aspx` w przeglądarce. Jak pokazano na rysunku 2, produkty z ceną w kategorii $20,00 mają wyróżnioną nazwę i cenę.

[![te produkty poniżej $20,00 są wyróżnione](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image5.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image4.png)

**Rysunek 2**. te produkty poniżej $20,00 są wyróżnione ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image6.png))

> [!NOTE]
> Ponieważ element DataList jest renderowany jako `<table>`HTML, jego wystąpienia `DataListItem` mają właściwości związane z stylem, które można ustawić w celu zastosowania określonego stylu do całego elementu. Na przykład jeśli chcemy wyróżnić *cały* element żółty, gdy jego cena była mniejsza niż $20,00, mogła zamienić kod, który odwołuje się do etykiet i ustawić `CssClass` właściwości w następującym wierszu kodu: `e.Item.CssClass = "AffordablePriceEmphasis"` (patrz rysunek 3).

`RepeaterItem` s, które tworzą formant wzmacniający, ale nie oferują takich właściwości na poziomie stylu. W związku z tym stosowanie niestandardowego formatowania do wzmacniania wymaga zastosowania właściwości stylu do kontrolek sieci Web w ramach szablonów wzmacniania, podobnie jak na rysunku 2.

[![cały produkt zostanie wyróżniony dla produktów poniżej $20,00](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image8.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image7.png)

**Rysunek 3**. cały element produktu został wyróżniony dla produktów w obszarze $20,00 ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image9.png))

## <a name="using-formatting-functions-from-within-the-template"></a>Używanie funkcji formatowania z poziomu szablonu

W samouczku *Używanie TemplateField w formancie GridView* pokazano, jak używać funkcji formatowania w widoku GridView TemplateField, aby zastosować niestandardowe formatowanie na podstawie danych powiązanych z wierszami GridView. Funkcja formatowania jest metodą, którą można wywołać z szablonu i zwraca kod HTML, który ma być emitowany w jego miejscu. Funkcje formatowania mogą znajdować się w klasie ASP.NET Page s lub mogą być scentralizowane do plików klas w folderze `App_Code` lub w oddzielnym projekcie biblioteki klas. Przeniesienie funkcji formatowania poza klasę ASP.NET strony powiązanej z kodem jest idealnym rozwiązaniem, jeśli planujesz używanie tej samej funkcji formatowania na wielu stronach ASP.NET lub w innych aplikacjach sieci Web ASP.NET.

Aby zademonstrować funkcje formatowania, niech s zawierają informacje o produkcie, a w przypadku, gdy nie jest to konieczne, jest to tekst [WYCOFANe] obok nazwy produktu. Ponadto poinformuj o tym, że cena została wyróżniona żółtym, jeśli liczba s jest mniejsza niż $20,00 (tak jak w przykładzie `ItemDataBound` obsługi zdarzeń); Jeśli cena ma wartość $20,00 lub wyższą, niech nie wyświetlaj rzeczywistej ceny, ale zamiast tekstu, zadzwoń do oferty cenowej. Na rysunku 4 przedstawiono zrzut ekranu przedstawiający listę produktów, których dotyczą te reguły formatowania.

[![kosztownych produktów, Cena jest zastępowana tekstem, wywołaj w celu uzyskania oferty cenowej](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image11.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image10.png)

**Ilustracja 4**. w przypadku kosztownych produktów cena jest zastępowana tekstem, wywołaj w celu uzyskania oferty cenowej ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image12.png))

## <a name="step-1-create-the-formatting-functions"></a>Krok 1. Tworzenie funkcji formatowania

Na potrzeby tego przykładu potrzebujemy dwóch funkcji formatowania, takich, które wyświetlają nazwę produktu wraz z tekstem [uncontinued], w razie potrzeby, a drugie, które wyświetla wyróżnioną cenę, jeśli nie jest ona mniejsza niż $20,00, lub tekst, w przeciwnym razie Wywołaj dla oferty cenowej. Zezwól programowi na tworzenie tych funkcji w klasie powiązanej z kodem ASP.NET strony i nadaj im nazwę `DisplayProductNameAndDiscontinuedStatus` i `DisplayPrice`. Obie metody muszą zwrócić kod HTML w celu renderowania jako ciąg i oba muszą być oznaczone `Protected` (lub `Public`), aby można było je wywołać ze ASP.NETej składni deklaracyjnej strony. Poniżej przedstawiono kod dla następujących dwóch metod:

[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample3.cs)]

Należy zauważyć, że metoda `DisplayProductNameAndDiscontinuedStatus` akceptuje wartości pól `productName` i `discontinued` danych jako wartości skalarnych, podczas gdy metoda `DisplayPrice` akceptuje wystąpienie `ProductsRow` (a nie `unitPrice` wartość skalarną). Jedno z tych rozwiązań będzie działało; Jeśli jednak funkcja formatowania działa z wartościami skalarnymi, które mogą zawierać wartości `NULL` bazy danych (takie jak `UnitPrice`; nie `ProductName` ani `Discontinued` zezwalać na `NULL` wartości), szczególną uwagę należy zwrócić w ramach obsługi tych skalarnych danych wejściowych.

W szczególności parametr wejściowy musi być typu `Object`, ponieważ wartością przychodzącą może być wystąpienie `DBNull` zamiast oczekiwanego typu danych. Ponadto należy przeprowadzić kontrolę, aby określić, czy wartość przychodząca jest wartością `NULL` bazy danych. Oznacza to, że jeśli chcemy, aby Metoda `DisplayPrice` akceptowała cenę jako wartość skalarną, należy użyć następującego kodu:

[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample4.cs)]

Należy zauważyć, że `unitPrice` parametr wejściowy jest typu `Object` i że Instrukcja warunkowa została zmodyfikowana w celu sprawdzenia, czy `unitPrice` jest `DBNull`, czy nie. Ponadto, ponieważ parametr wejściowy `unitPrice` jest przenoszona jako `Object`, musi być rzutowany do wartości dziesiętnej.

## <a name="step-2-calling-the-formatting-function-from-the-datalist-s-itemtemplate"></a>Krok 2: wywoływanie funkcji formatowania z elementu DataList s ItemTemplate

Za pomocą funkcji formatowania dodanych do naszej klasy ASP.NET strony powiązanej z kodem, wszystkie, które pozostają, są wywoływane te funkcje formatowania z `ItemTemplate`DataList. Aby wywołać funkcję formatowania z szablonu, umieść wywołanie funkcji w ramach składni DataBinding:

[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample5.aspx)]

W obszarze DataList s `ItemTemplate` kontrolka sieci Web `ProductNameLabel` etykieta Wyświetla obecnie nazwę produktu, przypisując jej Właściwość `Text` wyniku `<%# Eval("ProductName") %>`. Aby wyświetlić nazwę i tekst [nieobsługiwane], w razie potrzeby zaktualizuj składnię deklaratywną, tak aby przypisze Właściwość `Text` wartość metody `DisplayProductNameAndDiscontinuedStatus`. W takim przypadku musimy przekazać nazwę produktu i wartości wycofane przy użyciu składni `Eval("columnName")`. `Eval` zwraca wartość typu `Object`, ale metoda `DisplayProductNameAndDiscontinuedStatus` oczekuje parametrów wejściowych typu `String` i `Boolean`; w związku z tym należy rzutować wartości zwracane przez metodę `Eval` na oczekiwane typy parametrów wejściowych, na przykład:

[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample6.aspx)]

Aby wyświetlić cenę, można po prostu ustawić właściwość `UnitPriceLabel` Label s `Text` na wartość zwróconą przez metodę `DisplayPrice`, podobnie jak w przypadku wyświetlania nazwy produktu i tekstu [uncontinued]. Jednak zamiast przekazywania w `UnitPrice` jako skalarnego parametru wejściowego, zamiast tego przejdziemy do całego wystąpienia `ProductsRow`:

[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample7.aspx)]

W przypadku wywołań funkcji formatowania na miejscu zapoznaj się z postępem w przeglądarce. Ekran powinien wyglądać podobnie do rysunku 5, a wycofane produkty, takie jak tekst [WYCOFANe], są wyceniane ponad $20,00, jeśli cena została zamieniona na tekst, zadzwoń do oferty cenowej.

[![kosztownych produktów, Cena jest zastępowana tekstem, wywołaj w celu uzyskania oferty cenowej](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image14.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image13.png)

**Rysunek 5**. w przypadku kosztownych produktów cena jest zastępowana tekstem, wywołaj w celu uzyskania oferty cenowej ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image15.png))

## <a name="summary"></a>Podsumowanie

Formatowanie zawartości formantu DataList lub wzmacniania na podstawie danych można wykonać przy użyciu dwóch technik. Pierwszą techniką jest utworzenie procedury obsługi zdarzeń dla zdarzenia `ItemDataBound`, co spowoduje, że każdy rekord w źródle danych jest powiązany z nowym `DataListItem` lub `RepeaterItem`. W obsłudze zdarzeń `ItemDataBound` można sprawdzić bieżące dane elementu, a następnie można zastosować formatowanie do zawartości szablonu lub, dla `DataListItem` s, do całego elementu.

Alternatywnie formatowanie niestandardowe może być zrealizowane za poorednictwem funkcji formatowania. Funkcja formatowania to metoda, która może być wywoływana z szablonów DataList lub wzmacniak, które zwracają kod HTML, aby emitować w jego miejscu. Często kod HTML zwracany przez funkcję formatowania jest określany przez wartości, które są powiązane z bieżącym elementem. Te wartości mogą być przekazywane do funkcji formatowania jako wartości skalarne lub przez przekazanie całego obiektu, który jest powiązany z elementem (na przykład wystąpieniem `ProductsRow`).

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci liderzy dla tego samouczka to Yaakov Ellis, Randy Schmidt i Liz Shulok. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](displaying-data-with-the-datalist-and-repeater-controls-cs.md)
> [dalej](showing-multiple-records-per-row-with-the-datalist-control-cs.md)

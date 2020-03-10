---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-cs
title: Filtrowanie wzorców/szczegółów przy użyciu elementu DropDownListC#() | Microsoft Docs
author: rick-anderson
description: W tym samouczku zobaczymy, jak wyświetlać rekordy główne w kontrolce DropDownList oraz szczegóły wybranego elementu listy w widoku GridView.
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 53e659cc-eefb-40c1-a1dc-559481c99443
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-cs
msc.type: authoredcontent
ms.openlocfilehash: 3ec549f9da7a2b3a021e77827f0039e6ae60b5c5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78528820"
---
# <a name="masterdetail-filtering-with-a-dropdownlist-c"></a>Filtrowanie rekordu głównego/szczegółów przy użyciu kontrolki DropDownList (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_7_CS.exe) lub [Pobierz plik PDF](master-detail-filtering-with-a-dropdownlist-cs/_static/datatutorial07cs1.pdf)

> W tym samouczku zobaczymy, jak wyświetlać rekordy główne w kontrolce DropDownList oraz szczegóły wybranego elementu listy w widoku GridView.

## <a name="introduction"></a>Wprowadzenie

Typowym typem raportu jest *raport wzorzec/szczegóły*, w którym raport rozpoczyna się od wyświetlania pewnego zestawu rekordów "Master". Następnie użytkownik może przejść do szczegółów jednego z rekordów głównych, co spowoduje wyświetlenie "szczegółów" tego rekordu głównego. Raporty wzorzec/szczegóły są idealnym wyborem w przypadku wizualizacji relacji jeden-do-wielu, takich jak raport pokazujący wszystkie kategorie, a następnie umożliwiający użytkownikowi wybranie określonej kategorii i wyświetlenie skojarzonych z nimi produktów. Ponadto raporty wzorzec/szczegóły są przydatne do wyświetlania szczegółowych informacji z szczególnie "szerokich" tabel (z wieloma kolumnami). Na przykład poziom "Master" w raporcie głównym/szczegółowym może zawierać tylko nazwę produktu i cenę jednostkową produktów w bazie danych, a przechodzenie do określonego produktu będzie zawierać dodatkowe pola produktu (kategorii, dostawcy, ilości na jednostkę itd.).

Istnieje wiele sposobów implementacji raportu wzorzec/szczegóły. W ten sposób i następne trzy samouczki będą wyglądały na różnych raportach głównych/szczegółowych. W tym samouczku zobaczymy, jak wyświetlać rekordy główne w [kontrolce DropDownList](https://msdn.microsoft.com/library/dtx91y0z.aspx) oraz szczegóły wybranego elementu listy w widoku GridView. W szczególności Raport główny/szczegółowy tego samouczka zawiera listę informacji o kategorii i o produkcie.

## <a name="step-1-displaying-the-categories-in-a-dropdownlist"></a>Krok 1. Wyświetlanie kategorii w DropDownList

Nasz Raport główny/szczegółowy wyświetli listę kategorii w DropDownList, z których produkty wybranego elementu listy są wyświetlane w dalszej części strony w widoku GridView. Pierwsze zadanie przed nami, a następnie, ma mieć kategorie wyświetlane w DropDownList. Otwórz stronę `FilterByDropDownList.aspx` w folderze `Filtering`, przeciągnij na DropDownList z przybornika do projektanta strony i ustaw jej Właściwość `ID` na `Categories`. Następnie kliknij link wybierz źródło danych z tagu inteligentnego DropDownList. Spowoduje to wyświetlenie Kreatora konfiguracji źródła danych.

[![określić źródła danych DropDownList](master-detail-filtering-with-a-dropdownlist-cs/_static/image2.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image1.png)

**Rysunek 1**. Określanie źródła danych DropDownList (kliknij,[Aby wyświetlić obraz o pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-cs/_static/image3.png))

Wybierz, aby dodać nowy element ObjectDataSource o nazwie `CategoriesDataSource`, który wywołuje metodę `GetCategories()` klasy `CategoriesBLL`.

[![dodać nowego elementu ObjectDataSource o nazwie CategoriesDataSource](master-detail-filtering-with-a-dropdownlist-cs/_static/image5.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image4.png)

**Rysunek 2**. Dodawanie nowego elementu ObjectDataSource o nazwie `CategoriesDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-cs/_static/image6.png))

[![wybrać użycie klasy CategoriesBLL](master-detail-filtering-with-a-dropdownlist-cs/_static/image8.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image7.png)

**Rysunek 3**. wybór użycia klasy `CategoriesBLL` (kliknij,[Aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-cs/_static/image9.png))

[![skonfigurować element ObjectDataSource do korzystania z metody GetCategories ()](master-detail-filtering-with-a-dropdownlist-cs/_static/image11.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image10.png)

**Ilustracja 4**. Konfigurowanie elementu ObjectDataSource do używania metody `GetCategories()` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-cs/_static/image12.png))

Po skonfigurowaniu elementu ObjectDataSource nadal musimy określić, jakie pole źródła danych ma być wyświetlane w DropDownList, a które należy skojarzyć z wartością dla elementu listy. Pole `CategoryName`, które ma być wyświetlane i `CategoryID`, jako wartość dla każdego elementu listy.

[![wyświetlać DropDownList pole CategoryName i używać IDkategorii jako wartości](master-detail-filtering-with-a-dropdownlist-cs/_static/image14.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image13.png)

**Rysunek 5**. DropDownList wyświetlić pola `CategoryName` i użyj `CategoryID` jako wartości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-cs/_static/image15.png))

W tym momencie mamy kontrolkę DropDownList, która jest wypełniana rekordami z tabeli `Categories` (wszystkie wykonane przez około 6 sekund). Rysunek 6 przedstawia postęp z tego względu, gdy jest wyświetlany za pomocą przeglądarki.

[![listy rozwijanej z bieżącymi kategoriami](master-detail-filtering-with-a-dropdownlist-cs/_static/image17.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image16.png)

**Ilustracja 6**. Lista rozwijana z bieżącymi kategoriami ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-cs/_static/image18.png))

## <a name="step-2-adding-the-products-gridview"></a>Krok 2. Dodawanie produktu GridView

Ostatnim krokiem w naszym raporcie głównym/szczegółowym jest wyświetlenie listy produktów skojarzonych z wybraną kategorią. Aby to osiągnąć, Dodaj widok GridView do strony i Utwórz nowy element ObjectDataSource o nazwie `productsDataSource`. `productsDataSource` kontrolują dane wycofane z metody `GetProductsByCategoryID(categoryID)` klasy `ProductsBLL`.

[![wybrać metodę GetProductsByCategoryID (IDKategorii)](master-detail-filtering-with-a-dropdownlist-cs/_static/image20.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image19.png)

**Rysunek 7**. wybierz metodę `GetProductsByCategoryID(categoryID)` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-cs/_static/image21.png))

Po wybraniu tej metody Kreator ObjectDataSource poprosi nas o wartość parametru *`categoryID`* metody. Aby użyć wartości wybranego `categories` elementu DropDownList Ustaw Źródło parametru na Control i ControlID, aby `Categories`.

[![ustawić dla parametru IDKategorii wartości kategorii DropDownList](master-detail-filtering-with-a-dropdownlist-cs/_static/image23.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image22.png)

**Ilustracja 8**. ustaw parametr *`categoryID`* na wartość `Categories` DropDownList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-cs/_static/image24.png))

Poświęć chwilę, aby zaewidencjonować nasz postęp w przeglądarce. Podczas pierwszego odwiedzania strony są wyświetlane te produkty należące do wybranej kategorii (napoje) (jak pokazano na rysunku 9), ale zmiana DropDownList nie aktualizuje danych. Wynika to z faktu, że w celu zaktualizowania widoku GridView musi wystąpić ogłoszenie zwrotne. Aby to osiągnąć, mamy dwie opcje (żadna z tych elementów nie wymaga pisania kodu):

- **Ustaw**[Właściwość autoogłaszania](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listcontrol.autopostback%28VS.80%29.aspx)DropDownList kategorii**na wartość true.** (Możesz to zrobić, zaznaczając opcję Włącz autoogłaszanie w tagu inteligentnym DropDownList). Spowoduje to wyzwolenie ogłaszania zwrotnego za każdym razem, gdy wybrany element DropDownList zostanie zmieniony przez użytkownika. W związku z tym, gdy użytkownik wybierze nową kategorię z DropDownList, nastąpi odświeżenie zwrotne, a widok GridView zostanie zaktualizowany o produkty dla nowo wybranej kategorii. (To jest podejście, które zostało użyte w tym samouczku).
- **Dodaj kontrolkę sieci Web przycisku obok DropDownList.** Ustaw jej Właściwość `Text` na wartość Refresh lub podobną. W tym podejściu użytkownik będzie musiał wybrać nową kategorię, a następnie kliknąć przycisk. Kliknięcie przycisku spowoduje odświeżenie i zaktualizowanie widoku GridView w celu wyświetlenia listy produktów wybranej kategorii.

Ilustracje 9 i 10 ilustrują Raport główny/szczegółowy w działaniu.

[![podczas pierwszego odwiedzania strony zostaną wyświetlone produkty napoje](master-detail-filtering-with-a-dropdownlist-cs/_static/image26.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image25.png)

**Rysunek 9**. podczas pierwszego odwiedzania strony są wyświetlane produkty napoje ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-cs/_static/image27.png))

[![wybranie nowego produktu (produkt) automatycznie spowoduje odświeżenie, zaktualizowanie widoku GridView](master-detail-filtering-with-a-dropdownlist-cs/_static/image29.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image28.png)

**Ilustracja 10**: wybranie nowego produktu (produkt) powoduje automatyczne wypróbowanie ogłaszania zwrotnego, aktualizowanie widoku GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-cs/_static/image30.png))

## <a name="adding-a----choose-a-category----list-item"></a>Dodawanie elementu listy "--Wybierz kategorię--"

Gdy po raz pierwszy zostanie odwiedzana strona `FilterByDropDownList.aspx` kategorii (napoje) pierwszego elementu listy DropDownList są domyślnie zaznaczone, pokazując produkty napoje w widoku GridView. Zamiast wyświetlać produkty z pierwszej kategorii, możemy chcieć zamiast tego wybrać element DropDownList, który ma postać podobną do "--Wybierz kategorię--".

Aby dodać nowy element listy do DropDownList, przejdź do okno Właściwości i kliknij wielokropek we właściwości `Items`. Dodaj nowy element listy z `Text` "--Wybierz kategorię--" i `Value` `-1`.

[![dodać--wybierz kategorię--element listy](master-detail-filtering-with-a-dropdownlist-cs/_static/image32.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image31.png)

**Ilustracja 11**. Dodawanie elementu--wybierz kategorię--([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-cs/_static/image33.png))

Alternatywnie możesz dodać element listy, dodając następujący znacznik do DropDownList:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-cs/samples/sample1.aspx)]

Ponadto musimy ustawić `AppendDataBoundItems` formantu DropDownList na wartość true, ponieważ w przypadku, gdy kategorie są powiązane z DropDownList z elementu ObjectDataSource, zastąpią wszystkie ręcznie dodane elementy listy, jeśli `AppendDataBoundItems` nie ma wartości true.

![Ustaw właściwość AppendDataBoundItems na wartość true](master-detail-filtering-with-a-dropdownlist-cs/_static/image34.png)

**Ilustracja 12**. ustaw właściwość `AppendDataBoundItems` na wartość true

Po wprowadzeniu tych zmian podczas pierwszego odwiedzania strony wybrana jest opcja "--Wybierz kategorię--" i nie są wyświetlane żadne produkty.

[![ładowania strony początkowej nie są wyświetlane żadne produkty](master-detail-filtering-with-a-dropdownlist-cs/_static/image36.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image35.png)

**Ilustracja 13**. na początkowym załadowaniu strony nie są wyświetlane żadne produkty ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-cs/_static/image37.png))

Nie są wyświetlane żadne produkty, gdy jest wybrany element listy "--Wybierz kategorię--", ponieważ jego wartość jest `-1` i w bazie danych nie ma żadnych produktów z `CategoryID` `-1`. Jeśli jest to zachowanie, które chcesz wykonać, należy to zrobić. Jeśli jednak chcesz wyświetlić *wszystkie* kategorie, gdy zostanie wybrany element listy "--Wybierz kategorię--", Wróć do klasy `ProductsBLL` i dostosuj metodę `GetProductsByCategoryID(categoryID)`, tak aby wywołuje metodę `GetProducts()`, jeśli wartość parametru przekazanie w *`categoryID`* jest mniejsza od zera:

[!code-csharp[Main](master-detail-filtering-with-a-dropdownlist-cs/samples/sample2.cs)]

Użyta technika jest podobna do metody, która została użyta w celu wyświetlenia wszystkich dostawców z powrotem w samouczku [parametrów deklaratywnych](../basic-reporting/declarative-parameters-cs.md) , ale w tym przykładzie używamy wartości `-1`, aby wskazać, że wszystkie rekordy mają być pobierane w przeciwieństwie do `null`. Wynika to z faktu, że parametr *`categoryID`* metody `GetProductsByCategoryID(categoryID)` oczekuje, że przekazano liczbę całkowitą, podczas gdy w samouczku parametrów deklaratywnych podano parametr wejściowy ciągu.

Rysunek 14 przedstawia zrzut ekranu `FilterByDropDownList.aspx`, gdy zaznaczona jest opcja "--Wybierz kategorię--". W tym miejscu wszystkie produkty są wyświetlane domyślnie, a użytkownik może zawęzić widok, wybierając określoną kategorię.

[![wszystkie produkty są teraz wyświetlane domyślnie](master-detail-filtering-with-a-dropdownlist-cs/_static/image39.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image38.png)

**Ilustracja 14**. wszystkie produkty są teraz wyświetlane domyślnie ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-cs/_static/image40.png))

## <a name="summary"></a>Podsumowanie

Podczas wyświetlania danych zależnych od hierarchii często pomocne jest prezentowanie danych przy użyciu raportów master/detail, z których użytkownik może rozpocząć Peru danych od góry hierarchii i przejść do szczegółów. W tym samouczku sprawdzono Tworzenie prostego raportu głównego/szczegółów pokazującego produkty wybranej kategorii. Zostało to osiągnięte przy użyciu DropDownList dla listy kategorii i widoku GridView dla produktów należących do wybranej kategorii.

W [następnym samouczku](master-detail-filtering-with-two-dropdownlists-cs.md) interfejs DropDownList został jeszcze jeden krok, przy użyciu dwóch kontrolek DropDownList.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Dalej](master-detail-filtering-with-two-dropdownlists-cs.md)

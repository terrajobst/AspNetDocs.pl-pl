---
uid: web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-cs
title: Przyciski niestandardowe w elemencie DataList i WzmacniakeC#() | Microsoft Docs
author: rick-anderson
description: W tym samouczku utworzymy interfejs, który używa przewodnika, aby wyświetlić listę kategorii w systemie, z każdą kategorią zawierającą przycisk pokazujący associ...
ms.author: riande
ms.date: 11/13/2006
ms.assetid: 1f42e332-78dc-438b-9e35-0c97aa0ad929
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-cs
msc.type: authoredcontent
ms.openlocfilehash: e8cb1054068327c25e057b6df1cc7506feec8d37
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78576392"
---
# <a name="custom-buttons-in-the-datalist-and-repeater-c"></a>Przyciski niestandardowe w kontrolkach DataList i Repeater (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_46_CS.exe) lub [Pobierz plik PDF](custom-buttons-in-the-datalist-and-repeater-cs/_static/datatutorial46cs1.pdf)

> W tym samouczku utworzymy interfejs, który używa wzmacniak do wyświetlania kategorii w systemie, z każdą kategorią zawierającą przycisk pokazujący skojarzone produkty przy użyciu formantu BulletedList.

## <a name="introduction"></a>Wprowadzenie

W poprzednich samouczkach DataList i wzmacniak siedemnastu utworzyliśmy zarówno przykłady tylko do odczytu, jak i edycję oraz usuwanie przykładów. Aby ułatwić edytowanie i usuwanie funkcji w elemencie DataList, dodaliśmy przyciski do `ItemTemplate` DataList, które po kliknięciu powodowały ogłaszanie zwrotne i zgłosiło zdarzenie DataList odpowiadające właściwości `CommandName` przycisku. Na przykład dodanie przycisku do `ItemTemplate` z wartością właściwości `CommandName` Edytuj powoduje, że element DataList s `EditCommand` wyzwalać na stronie ogłaszania zwrotnego; jeden z `CommandName` Usuń podnosi `DeleteCommand`.

Oprócz przycisków Edytuj i Usuń, formanty DataList i wzmacniak mogą również zawierać przyciski, LinkButtons lub ImageButtons, które po kliknięciu wykonują niestandardową logikę po stronie serwera. W tym samouczku utworzymy interfejs, który używa przewodnika, aby wyświetlić listę kategorii w systemie. Dla każdej kategorii wzmacniak będzie zawierać przycisk umożliwiający wyświetlenie produktów skojarzonych z kategorią za pomocą kontrolki BulletedList (patrz rysunek 1).

[![kliknięciu linku Pokaż produkty są wyświetlane produkty kategorii z listy punktowanej](custom-buttons-in-the-datalist-and-repeater-cs/_static/image2.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image1.png)

**Rysunek 1**. kliknięcie linku Pokaż produkty powoduje wyświetlenie kategorii produktów na liście punktowanej ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](custom-buttons-in-the-datalist-and-repeater-cs/_static/image3.png))

## <a name="step-1-adding-the-custom-button-tutorial-web-pages"></a>Krok 1. Dodawanie stron sieci Web samouczka przycisku niestandardowego

Zanim sprawdzimy, jak dodać przycisk niestandardowy, poświęć chwilę na utworzenie stron ASP.NET w naszym projekcie witryny sieci Web, które będziemy potrzebować do tego samouczka. Zacznij od dodania nowego folderu o nazwie `CustomButtonsDataListRepeater`. Następnie Dodaj następujące dwie strony ASP.NET do tego folderu, aby upewnić się, że każda strona jest skojarzona z `Site.master` stroną wzorcową:

- `Default.aspx`
- `CustomButtons.aspx`

![Dodaj strony ASP.NET dla samouczków związanych z przyciskami niestandardowymi](custom-buttons-in-the-datalist-and-repeater-cs/_static/image4.png)

**Rysunek 2**. dodawanie stron ASP.NET dla samouczków związanych z przyciskami niestandardowymi

Podobnie jak w przypadku innych folderów, `Default.aspx` w folderze `CustomButtonsDataListRepeater` wyświetli samouczki w sekcji. Odwołaj się do tego, że formant użytkownika `SectionLevelTutorialListing.ascx` udostępnia tę funkcję. Dodaj tę kontrolkę użytkownika do `Default.aspx`, przeciągając ją z Eksplorator rozwiązań na stronę widok Projekt strony.

[![dodać kontrolkę użytkownika SectionLevelTutorialListing. ascx do default. aspx](custom-buttons-in-the-datalist-and-repeater-cs/_static/image6.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image5.png)

**Rysunek 3**. Dodawanie kontrolki użytkownika `SectionLevelTutorialListing.ascx` do `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](custom-buttons-in-the-datalist-and-repeater-cs/_static/image7.png))

Na koniec Dodaj strony jako wpisy do pliku `Web.sitemap`. W tym celu Dodaj następujące znaczniki po stronicowaniu i sortowaniu z `<siteMapNode>`DataList i wzmacniak:

[!code-xml[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample1.xml)]

Po aktualizacji `Web.sitemap`zapoznaj się z witryną internetową samouczków za pomocą przeglądarki. Menu po lewej stronie zawiera teraz elementy do edycji, wstawiania i usuwania samouczków.

![Mapa witryny zawiera teraz wpis do samouczka dotyczącego przycisków niestandardowych](custom-buttons-in-the-datalist-and-repeater-cs/_static/image8.png)

**Rysunek 4**. Mapa witryny zawiera teraz wpis do samouczka dotyczącego przycisków niestandardowych

## <a name="step-2-adding-the-list-of-categories"></a>Krok 2. Dodawanie listy kategorii

W tym samouczku musimy utworzyć wzmacniak, który wyświetla listę wszystkich kategorii wraz z element LinkButton produktami, które po kliknięciu wyświetla skojarzone produkty kategorii s na liście punktowanej. Pozwól, aby najpierw utworzyć prosty wzmacniak, który wyświetla listę kategorii w systemie. Zacznij od otwarcia strony `CustomButtons.aspx` w folderze `CustomButtonsDataListRepeater`. Przeciągnij wzmacniak z przybornika do projektanta i ustaw jego właściwość `ID` na `Categories`. Następnie utwórz nową kontrolkę źródła danych z tagu inteligentnego wzmacniak. W celu utworzenia nowej kontrolki ObjectDataSource o nazwie `CategoriesDataSource`, która wybiera swoje dane z metody `CategoriesBLL` klasy s `GetCategories()`.

[![skonfigurować element ObjectDataSource do używania metody GetCategories () klasy CategoriesBLL](custom-buttons-in-the-datalist-and-repeater-cs/_static/image10.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image9.png)

**Rysunek 5**. Konfigurowanie elementu ObjectDataSource do używania metody `GetCategories()` `CategoriesBLL` klasy s ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](custom-buttons-in-the-datalist-and-repeater-cs/_static/image11.png))

W przeciwieństwie do formantu DataList, dla którego program Visual Studio tworzy domyślne `ItemTemplate` w oparciu o źródło danych, należy ręcznie zdefiniować szablony wzmacniania. Ponadto szablony Wzmacniake muszą być tworzone i edytowane w sposób deklaratywny (to oznacza, że w tagu inteligentnym nie ma opcji Edytuj szablony).

Kliknij kartę Źródło w lewym dolnym rogu i Dodaj `ItemTemplate`, która wyświetla nazwę kategorii s w elemencie `<h3>` i jego opis w znaczniku akapitu. Dołącz `SeparatorTemplate`, w którym jest wyświetlana linia pozioma (`<hr />`) między każdą kategorią. Dodaj także element LinkButton z właściwością `Text`, aby wyświetlić produkty. Po wykonaniu tych kroków znaczniki deklaratywne strony powinny wyglądać następująco:

[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample2.aspx)]

Rysunek 6 przedstawia stronę wyświetlaną w przeglądarce. Nazwa kategorii i opis są wymienione na liście. Po kliknięciu przycisku Pokaż produkty zostanie wyświetlona pozycja ogłaszania zwrotnego, ale jeszcze nie wykonano żadnej akcji.

[![zostanie wyświetlona nazwa i opis kategorii s wraz z pokazywaniem produktów element LinkButton](custom-buttons-in-the-datalist-and-repeater-cs/_static/image13.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image12.png)

**Ilustracja 6**. wyświetlana jest nazwa i opis każdej kategorii, a także element LinkButton produkty ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](custom-buttons-in-the-datalist-and-repeater-cs/_static/image14.png))

## <a name="step-3-executing-server-side-logic-when-the-show-products-linkbutton-is-clicked"></a>Krok 3. wykonywanie logiki po stronie serwera po kliknięciu przycisku Pokaż produkty element LinkButton

Za każdym razem, gdy zostanie kliknięty przycisk, element LinkButton lub kliknięto element ImageButton w ramach szablonu w elemencie DataList lub Wzmacniake, zostanie wykonane ogłaszanie odświeżenia i zdarzenie DataList `ItemCommand`. Oprócz zdarzenia `ItemCommand`, formant DataList może również wywołać inne, bardziej szczegółowe zdarzenie, jeśli właściwość `CommandName` przycisku jest ustawiona na jeden z zastrzeżonych ciągów (usuwanie, edytowanie, anulowanie, aktualizowanie lub wybieranie), ale zdarzenie `ItemCommand` jest *zawsze* uruchamiane.

Gdy przycisk zostanie kliknięty w obrębie elementu DataList lub wzmacniak, często musimy się dowiedzieć, który przycisk został kliknięty (w przypadku, gdy w kontrolce może znajdować się wiele przycisków, takich jak przycisk Edytuj i Usuń), a także kilka dodatkowych informacji (takich jak wartość klucza podstawowego elementu, którego przycisk został kliknięty. Przyciski, element LinkButton i kliknięto element ImageButton zapewniają dwie właściwości, których wartości są przesyłane do programu obsługi zdarzeń `ItemCommand`:

- `CommandName` ciąg zwykle używany do identyfikowania każdego przycisku w szablonie
- `CommandArgument` często używane do przechowywania wartości niektórych pól danych, takich jak wartość klucza podstawowego

Na potrzeby tego przykładu należy ustawić właściwość element LinkButton `CommandName` s na ShowProducts i powiązać bieżącą wartość klucza podstawowego rekordu s `CategoryID` z właściwością `CommandArgument` przy użyciu składni wiązania danych `CategoryArgument='<%# Eval("CategoryID") %>'`. Po określeniu tych dwóch właściwości, składnia deklaracyjne element LinkButton powinna wyglądać następująco:

[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample3.aspx)]

Gdy przycisk zostanie kliknięty, następuje ogłaszanie zwrotne i zdarzenie DataList `ItemCommand`. Program obsługi zdarzeń przekazuje wartości `CommandName` i `CommandArgument` przycisku.

Utwórz procedurę obsługi zdarzeń dla zdarzenia `ItemCommand` wzmacniak i zanotuj drugi parametr przekazaną do procedury obsługi zdarzeń (o nazwie `e`). Ten drugi parametr jest typu [`RepeaterCommandEventArgs`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeatercommandeventargs.aspx) i ma cztery następujące właściwości:

- `CommandArgument` wartość właściwości `CommandArgument` klikniętego przycisku
- `CommandName` wartość właściwości `CommandName` przycisku.
- `CommandSource` odwołanie do kontrolki przycisku, który został kliknięty
- `Item` odwołanie do [`RepeaterItem`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem.aspx) zawierające kliknięty przycisk; Każdy rekord powiązany z wzmacniak jest uznawany za `RepeaterItem`

Ponieważ wybrana kategoria s `CategoryID` jest przenoszona za pośrednictwem właściwości `CommandArgument`, możemy pobrać zestaw produktów skojarzonych z wybraną kategorią w programie obsługi zdarzeń `ItemCommand`. Te produkty można następnie powiązać z kontrolką BulletedList w `ItemTemplate` (które jeszcze nie zostały dodane). Wszystko to, to znaczy dodać BulletedList, odwoływać się do niego w obsłudze zdarzeń `ItemCommand` i powiązać z nim zestaw produktów dla wybranej kategorii, którą będziemy w kroku 4.

> [!NOTE]
> Procedura obsługi zdarzeń `ItemCommand` DataList s jest przenoszona do obiektu typu [`DataListCommandEventArgs`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistcommandeventargs.aspx), który oferuje te same cztery właściwości, co Klasa `RepeaterCommandEventArgs`.

## <a name="step-4-displaying-the-selected-category-s-products-in-a-bulleted-list"></a>Krok 4. Wyświetlanie wybranych produktów kategorii z listy punktowanej

Wybrane produkty kategorii s mogą być wyświetlane w ramach `ItemTemplate`ka, przy użyciu dowolnej liczby kontrolek. Możemy dodać innego, zagnieżdżony wzmacniak, element DataList, DropDownList, GridView i tak dalej. Ponieważ chcemy wyświetlić produkty jako listę punktowaną, będziemy używać kontrolki BulletedList. Powracanie do znacznika deklaracyjnego `CustomButtons.aspx` strony, Dodaj kontrolkę BulletedList do `ItemTemplate` po element LinkButton Pokaż produkty. Ustaw `ID` BulletedLists s, aby `ProductsInCategory`. BulletedList wyświetla wartość pola danych określonego za pośrednictwem właściwości `DataTextField`; ponieważ ta kontrolka będzie miała powiązane informacje o produkcie, ustaw właściwość `DataTextField` na `ProductName`.

[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample4.aspx)]

W obsłudze zdarzeń `ItemCommand` należy odwołać się do tej kontrolki przy użyciu `e.Item.FindControl("ProductsInCategory")` i powiązać ją z zestawem produktów skojarzonych z wybraną kategorią.

[!code-csharp[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample5.cs)]

Przed wykonaniem jakiejkolwiek akcji w obsłudze zdarzeń `ItemCommand` należy najpierw sprawdzić wartość `CommandName`przychodzącego. Ponieważ program obsługi zdarzeń `ItemCommand` jest uruchamiany po kliknięciu *dowolnego* przycisku, jeśli w szablonie istnieje wiele przycisków, użyj wartości `CommandName`, aby rozpoznać działanie. Sprawdzanie `CommandName` w tym miejscu to Moot, ponieważ mamy tylko jeden przycisk, ale jest to dobry wykonywać do formularza. Następnie `CategoryID` wybranej kategorii zostanie pobrana z właściwości `CommandArgument`. Kontrolka BulletedList w szablonie jest następnie przywoływana i powiązana z wynikami metody `GetProductsByCategoryID(categoryID)` klasy `ProductsBLL`.

W poprzednich samouczkach, które używały przycisków w obrębie elementu DataList, takich jak [Omówienie edytowania i usuwania danych w elemencie DataList](../editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md), określamy wartość klucza podstawowego danego element za pośrednictwem kolekcji `DataKeys`. Chociaż to podejście działa dobrze z elementem DataList, wzmacniak nie ma właściwości `DataKeys`. Zamiast tego należy użyć alternatywnego podejścia do dostarczania wartości klucza podstawowego, na przykład przez właściwość Button s `CommandArgument` lub przez przypisanie wartości klucza podstawowego do kontrolki sieci Web ukrytej etykiety w ramach szablonu i odczytywanie jej wartości z powrotem w programie obsługi zdarzeń `ItemCommand` przy użyciu `e.Item.FindControl("LabelID")`.

Po zakończeniu procedury obsługi zdarzeń `ItemCommand` Poświęć chwilę na przetestowanie tej strony w przeglądarce. Jak pokazano na rysunku 7, kliknięcie linku Pokaż produkty powoduje ogłoszenie zwrotne i wyświetlenie produktów dla wybranej kategorii w BulletedList. Ponadto należy zauważyć, że te informacje o produkcie pozostaną, nawet jeśli inne kategorie pokazują linki produktów.

> [!NOTE]
> Jeśli chcesz zmodyfikować zachowanie tego raportu, w taki sposób, że jedyne produkty kategorii s są wyświetlane w danym momencie, po prostu ustaw właściwość BulletedList Control s `EnableViewState` na `False`.

[![BulletedList jest używany do wyświetlania produktów wybranej kategorii](custom-buttons-in-the-datalist-and-repeater-cs/_static/image16.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image15.png)

**Rysunek 7**. BulletedList jest używany do wyświetlania produktów wybranej kategorii ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](custom-buttons-in-the-datalist-and-repeater-cs/_static/image17.png))

## <a name="summary"></a>Podsumowanie

Formanty DataList i wzmacniak mogą zawierać dowolną liczbę przycisków, LinkButtons lub ImageButtons w swoich szablonach. Takie przyciski po kliknięciu powodują wystąpienie zwrotne i zgłoś zdarzenie `ItemCommand`. Aby skojarzyć niestandardową akcję po stronie serwera z klikniętym przyciskiem, Utwórz procedurę obsługi zdarzeń dla zdarzenia `ItemCommand`. W tym obsłudze zdarzeń należy najpierw sprawdzić wartość przychodzącą `CommandName`, aby określić, który przycisk został kliknięty. Dodatkowe informacje można opcjonalnie dostarczyć za pomocą właściwości `CommandArgument` przycisku.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzent potencjalnych klientów dla tego samouczka został Dennis Patterson. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Dalej](custom-buttons-in-the-datalist-and-repeater-vb.md)

---
uid: web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-vb
title: Korzystanie z szablonów FormView (VB) | Microsoft Docs
author: rick-anderson
description: W przeciwieństwie do widoku DetailsView, FormView nie składa się z pól. Zamiast tego, FormView jest renderowany przy użyciu szablonów. W tym samouczku będziemy analizować przy użyciu języka F...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 67b25f4c-2823-42b6-b07d-1d650b3fd711
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-vb
msc.type: authoredcontent
ms.openlocfilehash: cafe47cf5766bb14503852ec6e9f305d1e6d426f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78531165"
---
# <a name="using-the-formviews-templates-vb"></a>Korzystanie z szablonów FormView (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_14_VB.exe) lub [Pobierz plik PDF](using-the-formview-s-templates-vb/_static/datatutorial14vb1.pdf)

> W przeciwieństwie do widoku DetailsView, FormView nie składa się z pól. Zamiast tego, FormView jest renderowany przy użyciu szablonów. W tym samouczku sprawdzimy użycie kontrolki FormView, aby przedstawić mniej sztywny sposób wyświetlania danych.

## <a name="introduction"></a>Wprowadzenie

W dwóch ostatnich samouczkach pokazano, jak dostosować dane wyjściowe formantów GridView i DetailsView przy użyciu używanie TemplateField. Używanie TemplateField Zezwalaj na wysoce dostosowane zawartość dla określonego pola, ale na końcu zarówno GridView, jak i DetailsView, mają boxy wygląd podobny do siatki. W przypadku wielu scenariuszy takie jak układ podobny do siatki jest idealne, ale czasami jest wymagana mniejsza ilość obrazu. Podczas wyświetlania pojedynczego rekordu, taki układ płynu jest możliwy przy użyciu kontrolki FormView.

W przeciwieństwie do widoku DetailsView, FormView nie składa się z pól. Nie można dodać elementu BoundField ani TemplateField do widoku FormView. Zamiast tego, FormView jest renderowany przy użyciu szablonów. Należy traktować element FormView jako kontrolkę DetailsView, która zawiera pojedynczy TemplateField. W widoku FormView obsługiwane są następujące szablony:

- `ItemTemplate` używany do renderowania określonego rekordu wyświetlanego w widoku FormView
- `HeaderTemplate` używany do określenia opcjonalnego wiersza nagłówka
- `FooterTemplate` używany do określenia opcjonalnego wiersza stopki
- `EmptyDataTemplate`, gdy `DataSource` FormView nie zawiera żadnych rekordów, `EmptyDataTemplate` jest używany zamiast `ItemTemplate` do renderowania znaczników kontrolki
- `PagerTemplate` można użyć do dostosowania interfejsu stronicowania dla FormView, dla których włączono stronicowanie
- `EditItemTemplate` / `InsertItemTemplate` służy do dostosowywania interfejsu edycji lub wstawiania interfejsu dla FormView, które obsługują takie funkcje

W tym samouczku sprawdzimy użycie kontrolki FormView, aby przedstawić mniej sztywny sposób wyświetlania produktów. Zamiast pól dla nazwy, kategorii, dostawcy i tak dalej, `ItemTemplate` FormView będzie wyświetlać te wartości przy użyciu kombinacji elementu nagłówka i `<table>` (patrz rysunek 1).

[![, że w widoku DetailsView widoczne są przerwy z układu przypominającego siatkę](using-the-formview-s-templates-vb/_static/image2.png)](using-the-formview-s-templates-vb/_static/image1.png)

**Rysunek 1**: element FormView jest podzielony na układ podobny do siatki widoczny w widoku DetailsView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-formview-s-templates-vb/_static/image3.png))

## <a name="step-1-binding-the-data-to-the-formview"></a>Krok 1. powiązanie danych z formantem FormView

Otwórz stronę `FormView.aspx` i przeciągnij FormView z przybornika do projektanta. Gdy po raz pierwszy dodasz FormView jako szare pole, nakazuje nam, że `ItemTemplate` jest wymagana.

[![widoku FormView nie można renderować w projektancie, dopóki nie zostanie podana ItemTemplate](using-the-formview-s-templates-vb/_static/image5.png)](using-the-formview-s-templates-vb/_static/image4.png)

**Rysunek 2**: formant FormView nie może być renderowany w projektancie, dopóki nie zostanie podana `ItemTemplate` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-the-formview-s-templates-vb/_static/image6.png))

`ItemTemplate` można utworzyć ręcznie (poprzez składnię deklaratywną) lub można ją utworzyć za pomocą powiązania widoku FormView z kontrolką źródła danych za pomocą projektanta. Ta `ItemTemplate` utworzona przez tę funkcję zawiera kod HTML, który wyświetla nazwę każdego pola i kontrolkę etykiety, której Właściwość `Text` jest powiązana z wartością pola. To podejście również automatycznie tworzy `InsertItemTemplate` i `EditItemTemplate`, z których oba są wypełniane kontrolkami wejściowymi dla każdego z pól danych zwracanych przez formant źródła danych.

Jeśli chcesz utworzyć szablon do autouzupełniania, w tagu inteligentnym FormView Dodaj nową kontrolkę ObjectDataSource, która wywołuje metodę `GetProducts()` klasy `ProductsBLL`. Spowoduje to utworzenie FormView z `ItemTemplate`, `InsertItemTemplate`i `EditItemTemplate`. W widoku źródła Usuń `InsertItemTemplate` i `EditItemTemplate`, ponieważ nie interesujemy się tworzeniem widoku FormView, który obsługuje edytowanie lub wstawianie. Następnie wyczyść znaczniki w `ItemTemplate` tak, aby można było z nich korzystać.

Jeśli wolisz utworzyć `ItemTemplate` ręcznie, możesz dodać i skonfigurować element ObjectDataSource, przeciągając go z przybornika do projektanta. Nie należy jednak ustawiać źródła danych FormView z projektanta. Zamiast tego przejdź do widoku źródła i ręcznie ustaw właściwość `DataSourceID` FormView na wartość `ID` elementu ObjectDataSource. Następnie ręcznie Dodaj `ItemTemplate`.

Niezależnie od tego, jaką metodę należy podjąć, w tym momencie deklaratywne znaczniki w FormView powinny wyglądać następująco:

[!code-aspx[Main](using-the-formview-s-templates-vb/samples/sample1.aspx)]

Poświęć chwilę, aby zaznaczyć pole wyboru Włącz stronicowanie w tagu inteligentnym FormView; spowoduje to dodanie atrybutu `AllowPaging="True"` do składni deklaratywnej FormView. Ponadto należy ustawić wartość false dla właściwości `EnableViewState`.

## <a name="step-2-defining-theitemtemplates-markup"></a>Krok 2. Definiowanie znaczników`ItemTemplate`

Przy użyciu kontrolki FormView powiązanej z kontrolką ObjectDataSource i skonfigurowanej do obsługi stronicowania gotowe do określenia zawartości dla `ItemTemplate`. Na potrzeby tego samouczka zostanie wyświetlona nazwa produktu w `<h3>` nagłówku. Poniżej można używać `<table>` HTML do wyświetlania właściwości pozostałych produktów w tabeli z czterema kolumnami, w której pierwsza i trzecia kolumna zawiera nazwy właściwości, a druga i czwarta ich wartości.

To oznakowanie można wprowadzić za pomocą interfejsu edycji szablonu FormView w projektancie lub wprowadzić ręcznie przy użyciu składni deklaracyjnej. Podczas pracy z szablonami zwykle znajdują się one szybciej, aby pracować bezpośrednio z składnią deklaratywną, ale możesz korzystać z dowolnej metody, która jest najbardziej wygodna.

Poniższe znaczniki przedstawiają znaczniki deklaratywne FormView po zakończeniu `ItemTemplate`strukturze:

[!code-aspx[Main](using-the-formview-s-templates-vb/samples/sample2.aspx)]

Zwróć uwagę, że składnia DataBinding-`<%# Eval("ProductName") %>`, na przykład, można wstrzyknąć bezpośrednio do danych wyjściowych szablonu. Oznacza to, że nie musi być przypisane do właściwości `Text` formantu etykiety. Na przykład mamy `ProductName` wartość, która jest wyświetlana w `<h3>` elemencie przy użyciu `<h3><%# Eval("ProductName") %></h3>`, która dla produktu Chai będzie renderowana jako `<h3>Chai</h3>`.

Klasy `ProductPropertyLabel` i `ProductPropertyValue` CSS są używane do określania stylu nazw i wartości właściwości produktu w `<table>`. Te klasy CSS są zdefiniowane w `Styles.css` i powodują, że nazwy właściwości mają być pogrubione i wyrównane do prawej i dodane do wartości właściwości.

Ponieważ nie ma CheckBoxFields dostępnych w widoku FormView, aby pokazać wartość `Discontinued` jako pole wyboru, należy dodać własną kontrolkę pola wyboru. Właściwość `Enabled` jest ustawiona na wartość false, co oznacza, że jest tylko do odczytu, a właściwość `Checked` pola wyboru jest powiązana z wartością pola dane `Discontinued`.

Po zakończeniu `ItemTemplate` informacje o produkcie są wyświetlane w sposób znacznie bardziej płynny. Porównaj dane wyjściowe DetailsView z ostatniego samouczka (rysunek 3) o dane wyjściowe generowane przez FormView w tym samouczku (rysunek 4).

[![sztywne dane wyjściowe DetailsView](using-the-formview-s-templates-vb/_static/image8.png)](using-the-formview-s-templates-vb/_static/image7.png)

**Rysunek 3**: sztywne wyjście DetailsView ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-the-formview-s-templates-vb/_static/image9.png))

[![danych wyjściowych płynu FormView](using-the-formview-s-templates-vb/_static/image11.png)](using-the-formview-s-templates-vb/_static/image10.png)

**Ilustracja 4**. dane wyjściowe w postaci danych wyjściowych FormView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-formview-s-templates-vb/_static/image12.png))

## <a name="summary"></a>Podsumowanie

Mimo że kontrolki GridView i DetailsView mogą mieć niestandardowe dane wyjściowe dostosowane przy użyciu używanie TemplateField, oba elementy nadal zawierają dane w formacie boxy, podobnie jak w postaci siatki. Dla tych czasów, gdy pojedynczy rekord musi być pokazywany przy użyciu mniej sztywnego układu, FormView jest idealnym wyborem. Podobnie jak w widoku DetailsView, FormView renderuje jeden rekord z `DataSource`, ale w przeciwieństwie do widoku DetailsView składa się tylko z szablonów i nie obsługuje pól.

Jak zostało to opisane w tym samouczku, FormView pozwala na bardziej elastyczny układ podczas wyświetlania pojedynczego rekordu. W przyszłych samouczkach sprawdzimy kontrolki DataList i wzmacniak, które zapewniają taki sam poziom elastyczności jak FormsView, ale mogą wyświetlać wiele rekordów (takich jak GridView).

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzent potencjalnych klientów dla tego samouczka został E.R. Gilmore. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](using-templatefields-in-the-detailsview-control-vb.md)
> [dalej](displaying-summary-information-in-the-gridview-s-footer-vb.md)

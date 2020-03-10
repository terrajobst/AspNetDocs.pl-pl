---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Wyświetlanie elementów danych i szczegółów | Microsoft Docs
author: Erikre
description: W tej serii samouczków przedstawiono podstawowe informacje na temat tworzenia aplikacji ASP.NET Web Forms z ASP.NET 4,7 i Microsoft Visual Studio 2017
ms.author: riande
ms.date: 1/04/2019
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 130c9ffd29df612dac5bb954830a2eb9b738aaf0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78641114"
---
# <a name="display-data-items-and-details"></a>Wyświetlanie elementów danych i szczegółów

Autor [Erik Reitan](https://github.com/Erikre)

> Ta seria samouczków uczy się podstaw tworzenia aplikacji ASP.NET Web Forms z ASP.NET 4,7 i Microsoft Visual Studio 2017.

W tym samouczku dowiesz się, jak wyświetlać elementy danych i szczegóły elementów danych za pomocą formularzy sieci Web ASP.NET i Code First Entity Framework. Ten samouczek jest oparty na poprzednim samouczku "interfejs użytkownika i Nawigacja" w ramach serii samouczków Wingtip. Po ukończeniu tego samouczka zobaczysz produkty na stronie *ProductsList. aspx* oraz szczegóły produktu na stronie *ProductDetails. aspx* .

## <a name="youll-learn-how-to"></a>Omawiane tematy:

- Dodaj kontrolkę danych, aby wyświetlić produkty z bazy danych
- Połącz formant danych z wybranymi danymi
- Dodaj kontrolkę danych, aby wyświetlić szczegóły produktu z bazy danych
- Pobierz wartość z ciągu zapytania i Użyj tej wartości, aby ograniczyć dane pobierane z bazy danych

### <a name="features-introduced-in-this-tutorial"></a>Funkcje wprowadzone w tym samouczku:

- Powiązanie modelu
- Dostawcy wartości

## <a name="add-a-data-control"></a>Dodawanie kontrolki danych

Możesz użyć kilku różnych opcji, aby powiązać dane z kontrolką serwera. Najpopularniejsze obejmują:

* Dodawanie kontrolki źródła danych
* Ręczne dodawanie kodu
* Korzystanie z powiązania modelu

### <a name="use-a-data-source-control-to-bind-data"></a>Używanie kontroli źródła danych do wiązania danych

Dodanie kontroli źródła danych umożliwia połączenie kontroli źródła danych z kontrolką wyświetlającą dane. Dzięki temu można deklaratywnie, a nie programowo, łączyć kontrolki po stronie serwera ze źródłami danych.

### <a name="code-by-hand-to-bind-data"></a>Zakoduj ręcznie, aby powiązać dane

Ręczne kodowanie obejmuje:

1. Odczytywanie wartości
2. Sprawdzanie, czy jest to wartość zerowa
3. Konwertowanie na odpowiedni typ
4. Sprawdzanie powodzenia konwersji
5. Używanie wartości w zapytaniu 

Takie podejście umożliwia pełną kontrolę nad logiką dostępu do danych.

### <a name="use-model-binding-to-bind-data"></a>Użyj powiązania modelu, aby powiązać dane

Powiązanie modelu pozwala powiązać wyniki z znacznie mniejszym kodem i umożliwia ponowne używanie funkcji w całej aplikacji. Upraszcza ona pracę z logiką dostępu do danych ukierunkowanych na kod, zapewniając jednocześnie rozbudowaną strukturę powiązań danych.

## <a name="display-products"></a>Wyświetl produkty

W tym samouczku użyjesz powiązania modelu, aby powiązać dane. Aby skonfigurować kontrolkę danych do używania powiązania modelu w celu wybrania danych, należy ustawić właściwość `SelectMethod` formantu na nazwę metody w kodzie strony. Kontrola danych wywołuje metodę w odpowiednim czasie w cyklu życia strony i automatycznie wiąże zwrócone dane. Nie trzeba jawnie wywoływać metody `DataBind`.

1. W **Eksplorator rozwiązań**Otwórz *ProductList. aspx*.
2. Zastąp istniejące znaczniki tym znacznikiem:   

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample1.aspx)]

Ten kod używa kontrolki **ListView** o nazwie `productList` do wyświetlania produktów.

[!code-aspx-csharp[Main](display_data_items_and_details/samples/sample2.aspx)]

Za pomocą szablonów i stylów definiujesz sposób wyświetlania danych w formancie **ListView** . Jest to przydatne w przypadku danych w każdej powtarzającej się strukturze. Chociaż ten **element ListView** przykład po prostu wyświetla dane bazy danych, można również bez kodu, umożliwić użytkownikom edytowanie, wstawianie i usuwanie danych oraz sortowanie i dane stronicowe.

Ustawienie właściwości `ItemType` w formancie **ListView** powoduje, że wyrażenie powiązania danych `Item` jest dostępne, a kontrolka będzie silnie wpisana. Jak wspomniano w poprzednim samouczku, można wybrać szczegóły obiektu elementu z technologią IntelliSense, na przykład określając `ProductName`:

![Wyświetlanie elementów danych i szczegółów — IntelliSense](display_data_items_and_details/_static/image1.png)

Używasz również powiązania modelu, aby określić wartość `SelectMethod`. Ta wartość (`GetProducts`) odnosi się do metody, która zostanie dodana do kodu, aby wyświetlić produkty w następnym kroku.

### <a name="add-code-to-display-products"></a>Dodaj kod, aby wyświetlić produkty

W tym kroku dodasz kod, aby wypełnić formant **ListView** danymi produktu z bazy danych. Kod obsługuje wyświetlanie wszystkich produktów i produktów poszczególnych kategorii.

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy *ProductList. aspx* , a następnie wybierz polecenie **Wyświetl kod**.
2. Zastąp istniejący kod w pliku *ProductList.aspx.cs* :   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

Ten kod przedstawia metodę `GetProducts`, do której odwołuje się Właściwość `ItemType` formantu **ListView** na stronie *ProductList. aspx* . Aby ograniczyć wyniki do określonej kategorii bazy danych, kod ustawia wartość `categoryId` z wartości ciągu zapytania przesłanej do strony *ProductList. aspx* po przejściu do strony *ProductList. aspx* . Klasa `QueryStringAttribute` w przestrzeni nazw `System.Web.ModelBinding` służy do pobierania wartości zmiennej ciągu zapytania `id`. Powoduje to, że powiązanie modelu próbuje powiązać wartość z ciągu zapytania do parametru `categoryId` w czasie wykonywania.

Gdy poprawna kategoria jest przenoszona jako ciąg zapytania do strony, wyniki zapytania są ograniczone do tych produktów w bazie danych, które pasują do wartości `categoryId`. Na przykład jeśli adres URL strony *ProductsList. aspx* jest następujący:

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

Na stronie są wyświetlane tylko te produkty, dla których `categoryId` jest równa `1`.

Wszystkie produkty są wyświetlane, jeśli nie zostanie uwzględniony ciąg zapytania po wywołaniu strony *ProductList. aspx* .

Źródła wartości dla tych metod są określane jako *dostawcy wartości* (takie jak *QueryString*) i atrybuty parametrów wskazujące, który dostawca wartości ma być używany, jest określany jako *atrybuty dostawcy wartości* (takie jak `id`). ASP.NET obejmuje dostawców wartości i odpowiadające im atrybuty dla wszystkich typowych źródeł danych wejściowych użytkownika w aplikacji formularzy sieci Web, takich jak ciąg zapytania, pliki cookie, wartości formularzy, kontrolki, stan widoku, stan sesji i właściwości profilu. Możesz również pisać dostawców wartości niestandardowych.

### <a name="run-the-application"></a>Uruchamianie aplikacji

Uruchom aplikację teraz, aby wyświetlić wszystkie produkty lub produkty kategorii.

1. Naciśnij klawisz **F5** w programie Visual Studio, aby uruchomić aplikację.  
   Zostanie otwarta przeglądarka i zostanie wyświetlona strona *default. aspx* .

2. Wybierz pozycję **samochody** z menu nawigacji kategorii produktów.  
   Na stronie *ProductList. aspx* wyświetlane są tylko produkty kategorii **samochodów** . W dalszej części tego samouczka zostaną wyświetlone szczegółowe informacje o produkcie.  

    ![Wyświetlanie elementów danych i szczegółów — samochody](display_data_items_and_details/_static/image2.png)

3. Wybierz pozycję **produkty** z menu nawigacji u góry.  
   Ponownie zostanie wyświetlona strona *ProductList. aspx* , jednak spowoduje to wyświetlenie całej listy produktów.   

    ![Wyświetlanie elementów danych i szczegółów — produkty](display_data_items_and_details/_static/image3.png)

4. Zamknij przeglądarkę i wróć do programu Visual Studio.

### <a name="add-a-data-control-to-display-product-details"></a>Dodaj kontrolkę danych, aby wyświetlić szczegóły produktu

Następnie zmodyfikujesz znaczniki na stronie *ProductDetails. aspx* , która została dodana w poprzednim samouczku, aby wyświetlić określone informacje o produkcie.

1. W **Eksplorator rozwiązań**Otwórz *ProductDetails. aspx*.

2. Zastąp istniejące znaczniki tym znacznikiem:

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample5.aspx)] 

    Ten kod używa kontrolki **FormView** do wyświetlania szczegółowych informacji o produkcie. Ten znacznik używa metod, takich jak metody używane do wyświetlania danych na stronie *ProductList. aspx* . Kontrolka **FormView** służy do wyświetlania pojedynczego rekordu w czasie ze źródła danych. Gdy używasz kontrolki **FormView** , tworzysz szablony, aby wyświetlać i edytować wartości powiązane z danymi. Szablony te zawierają kontrolki, wyrażenia powiązań i formatowanie, które definiują wygląd i funkcjonalność formularza.

Połączenie z poprzednim znacznikiem do bazy danych wymaga dodatkowego kodu.

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy *ProductDetails. aspx* , a następnie kliknij polecenie **Wyświetl kod**.  
   Zostanie wyświetlony plik *ProductDetails.aspx.cs* .

2. Zastąp istniejący kod tym kodem:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

Ten kod sprawdza wartość ciągu zapytania "`productID`". Jeśli zostanie znaleziona prawidłowa wartość ciągu zapytania, zostanie wyświetlony pasujący produkt. Jeśli nie można odnaleźć ciągu zapytania lub jego wartość jest nieprawidłowa, nie jest wyświetlany żaden produkt.

### <a name="run-the-application"></a>Uruchamianie aplikacji

Teraz możesz uruchomić aplikację, aby zobaczyć pojedynczy produkt wyświetlany na podstawie identyfikatora produktu.

1. Naciśnij klawisz **F5** w programie Visual Studio, aby uruchomić aplikację.  
   Zostanie otwarta przeglądarka i zostanie wyświetlona strona *default. aspx* .

2. Wybierz pozycję **łodzie** w menu nawigacji kategorii.  
   Zostanie wyświetlona strona *ProductList. aspx* .

3. Wybierz pozycję **papierowa łodzi** z listy produkt.
   Zostanie wyświetlona strona *ProductDetails. aspx* .

    ![Wyświetlanie elementów danych i szczegółów — produkty](display_data_items_and_details/_static/image4.png)
    
4. Zamknij okno przeglądarki.

## <a name="additional-resources"></a>Dodatkowe zasoby

[Pobieranie i wyświetlanie danych przy użyciu powiązania modelu i formularzy sieci Web](../../presenting-and-managing-data/model-binding/retrieving-data.md)

## <a name="next-steps"></a>Następne kroki

W tym samouczku dodano znaczniki i kod, aby wyświetlić produkty i informacje o produkcie. Informacje o silnych typach kontroli danych, powiązaniach modelu i dostawcach wartości. W następnym samouczku dodasz koszyk do przykładowej aplikacji Wingtip zabawki. 

> [!div class="step-by-step"]
> [Poprzednie](ui_and_navigation.md)
> [dalej](shopping-cart.md)

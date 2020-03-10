---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
title: Wykonywanie prostej weryfikacjiC#() | Microsoft Docs
author: StephenWalther
description: Dowiedz się, jak przeprowadzić walidację w aplikacji ASP.NET MVC. W tym samouczku Stephen Walther wprowadza do modelu stan i pomocnik HTML walidacji...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 21383c9d-6aea-4bad-a99b-b5f2c9d6503f
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
msc.type: authoredcontent
ms.openlocfilehash: e33f522af74efe97b5a245e956bc0b918ea769af
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78542995"
---
# <a name="performing-simple-validation-c"></a>Wykonywanie prostej walidacji (C#)

Autor [Stephen Walther](https://github.com/StephenWalther)

> Dowiedz się, jak przeprowadzić walidację w aplikacji ASP.NET MVC. W tym samouczku Stephen Walther wprowadza do modelu stanu i pomocników HTML weryfikacji.

Celem tego samouczka jest wyjaśnienie, jak można przeprowadzić walidację w aplikacji ASP.NET MVC. Na przykład dowiesz się, jak uniemożliwić komuś przesłanie formularza, który nie zawiera wartości dla wymaganego pola. Dowiesz się, jak korzystać ze stanu modelu i pomocników HTML walidacji.

## <a name="understanding-model-state"></a>Informacje o stanie modelu

Używasz stanu modelu — lub dokładniej, słownika stanu modelu — do reprezentowania błędów walidacji. Na przykład akcja Utwórz () na liście 1 sprawdza poprawność właściwości klasy produktu przed dodaniem klasy produktu do bazy danych.

Nie zaleca się dodawania walidacji lub logiki bazy danych do kontrolera. Kontroler powinien zawierać tylko logikę powiązaną z kontrolą przepływu aplikacji. Wykonujemy skrót, aby zachować prostotę.

**Lista 1 — Controllers\ProductController.cs**

[!code-csharp[Main](performing-simple-validation-cs/samples/sample1.cs)]

Na liście 1 są sprawdzane nazwy, opisy i właściwości UnitsInStock klasy Product. Jeśli którakolwiek z tych właściwości zakończy się niepowodzeniem testu weryfikacyjnego, zostanie dodany błąd do słownika stanu modelu (reprezentowane przez właściwość ModelState klasy kontrolera).

W przypadku wystąpienia błędów w stanie modelu Właściwość ModelState. IsValid zwraca wartość false. W takim przypadku formularz HTML służący do tworzenia nowego produktu jest ponownie wyświetlany. W przeciwnym razie, jeśli nie występują błędy walidacji, nowy produkt zostanie dodany do bazy danych.

## <a name="using-the-validation-helpers"></a>Korzystanie z pomocników weryfikacji

Struktura ASP.NET MVC obejmuje dwa pomocnicy weryfikacji: pomocnik html. ValidationMessage () oraz pomocnik html. podsumowania walidacji (). Te dwa pomocnicy są używane w widoku do wyświetlania komunikatów o błędach walidacji.

Pomocników html. ValidationMessage () i HTML. podsumowania walidacji () są używane w widokach tworzenie i edytowanie, które są generowane automatycznie przez szkielet ASP.NET MVC. Wykonaj następujące kroki, aby wygenerować widok tworzenia:

1. Kliknij prawym przyciskiem myszy akcję Utwórz () w kontrolerze produktu i wybierz opcję menu **Dodaj widok** (patrz rysunek 1).
2. W oknie dialogowym **Dodawanie widoku** zaznacz pole wyboru z etykietą **Utwórz widok o jednoznacznie określonym typie** (patrz rysunek 2).
3. Z listy rozwijanej **Wyświetl klasę danych** wybierz klasę Product (produkt).
4. Z listy rozwijanej **Wyświetl zawartość** wybierz pozycję Utwórz.
5. Kliknij przycisk **Dodaj**.

Przed dodaniem widoku upewnij się, że aplikacja została utworzona. W przeciwnym razie Lista klas nie zostanie wyświetlona na liście rozwijanej **klasy Wyświetl dane** .

[![okno dialogowe Nowy projekt](performing-simple-validation-cs/_static/image1.jpg)](performing-simple-validation-cs/_static/image1.png)

**Ilustracja 01**. Dodawanie widoku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](performing-simple-validation-cs/_static/image2.png))

[![okno dialogowe Nowy projekt](performing-simple-validation-cs/_static/image2.jpg)](performing-simple-validation-cs/_static/image3.png)

**Ilustracja 02**. Tworzenie widoku o jednoznacznie określonym typie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](performing-simple-validation-cs/_static/image4.png))

Po wykonaniu tych kroków zostanie wyświetlony widok tworzenie na liście 2.

**Lista 2 — Views\Product\Create.aspx**

[!code-aspx[Main](performing-simple-validation-cs/samples/sample2.aspx)]

W przypadku listy 2 pomocnik html. podsumowania walidacji () jest wywoływany bezpośrednio nad formularzem HTML. Ten pomocnik służy do wyświetlania listy komunikatów o błędach walidacji. Pomocnik html. podsumowania walidacji () renderuje błędy na liście punktowanej.

Pomocnik html. ValidationMessage () jest wywoływana obok każdego z pól formularza HTML. Ten pomocnik służy do wyświetlania komunikatu o błędzie bezpośrednio obok pola formularza. W przypadku listy 2 pomocnik html. ValidationMessage () wyświetla gwiazdkę, gdy wystąpi błąd.

Strona na rysunku 3 ilustruje komunikaty o błędach renderowane przez pomocników weryfikacji podczas przesyłania formularza z brakującymi polami i nieprawidłowymi wartościami.

[![okno dialogowe Nowy projekt](performing-simple-validation-cs/_static/image3.jpg)](performing-simple-validation-cs/_static/image5.png)

**Ilustracja 03**: widok tworzenia przesłany z problemami ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](performing-simple-validation-cs/_static/image6.png))

Zauważ, że wygląd pól danych wejściowych HTML jest również modyfikowany w przypadku błędu walidacji. Pomocnik html. TextBox () renderuje atrybut *Class = "Input-Validation-Error"* w przypadku błędu walidacji skojarzonego z właściwością renderowaną przez pomocnika html. TextBox ().

Istnieją trzy kaskadowe klasy arkuszy stylów służące do kontrolowania wyglądu błędów walidacji:

- dane wejściowe-Walidacja-błąd — zastosowano do tagu &lt;Input&gt; renderowanego przez pomocnika html. TextBox ().
- pole-Walidacja-błąd — zastosowano do tagu &lt;span&gt; renderowanego przez pomocnika html. ValidationMessage ().
- Walidacja-Summary-Errors-zastosowano do tagu &lt;ul&gt; renderowanego przez pomocnika html. podsumowania walidacji ().

Można modyfikować te kaskadowe klasy arkuszy stylów i w związku z tym modyfikować wygląd błędów sprawdzania poprawności, modyfikując plik site. css znajdujący się w folderze Content.

> [!NOTE] 
> 
> Klasa HtmlHelper zawiera statyczne właściwości tylko do odczytu na potrzeby pobierania nazw klas CSS związanych z walidacją. Te właściwości statyczne mają nazwy ValidationInputCssClassName, ValidationFieldCssClassName i ValidationSummaryCssClassName.

## <a name="prebinding-validation-and-postbinding-validation"></a>Walidacja prepowiązania i walidacja po powiązaniu

Jeśli przesyłasz formularz HTML do tworzenia produktu, a wprowadzisz nieprawidłową wartość pola Cena i nie wartość pola UnitsInStock, otrzymasz komunikaty weryfikacyjne wyświetlane na rysunku 4. Skąd pochodzą te komunikaty o błędach walidacji?

[![okno dialogowe Nowy projekt](performing-simple-validation-cs/_static/image4.jpg)](performing-simple-validation-cs/_static/image7.png)

**Rysunek 04**: Błędy walidacji dotyczące powiązań ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](performing-simple-validation-cs/_static/image8.png))

Istnieją w rzeczywistości dwa typy komunikatów o błędach walidacji — te wygenerowane przed polami formularza HTML są powiązane z klasą, a te, które zostały wygenerowane po wykonaniu pól formularza, są powiązane z klasą. Innymi słowy występują błędy walidacji i błędy walidacji po wystąpieniu powiązania.

Akcja Create () udostępniona przez kontroler produktu na liście 1 akceptuje wystąpienie klasy Product. Podpis metody Create wygląda następująco:

[!code-csharp[Main](performing-simple-validation-cs/samples/sample3.cs)]

Wartości pól formularza HTML z formularza Create są powiązane z klasą productToCreate przez coś nazywanego spinaczem modelu. Model segregatora domyślnego dodaje komunikat o błędzie do stanu modelu automatycznie, gdy nie można powiązać pola formularza z właściwością form.

Spinacz modelu domyślnego nie może powiązać ciągu "Apple" z właściwością Price klasy Product. Nie można przypisać ciągu do właściwości dziesiętnej. W związku z tym spinacz modelu dodaje błąd do modelu stanu.

Model segregatora domyślnego nie może również przypisać wartości null do właściwości, która nie akceptuje wartości null. W szczególności spinacz modelu nie może przypisać wartości null do właściwości UnitsInStock. Po ponownym utworzeniu spinacza modelu przypisuje i dodaje komunikat o błędzie do stanu modelu.

Jeśli chcesz dostosować wygląd tych, które wiążą się z tymi komunikatami o błędach, należy utworzyć ciągi zasobów dla tych komunikatów.

## <a name="summary"></a>Podsumowanie

Celem tego samouczka było opisywanie podstawowych Mechanics weryfikacji w strukturze ASP.NET MVC. Wiesz już, jak korzystać ze stanu modelu i pomocników HTML weryfikacji. Omawiamy również różnice między instrukcją Prebind i walidacją po powiązaniu. W innych samouczkach omówiono różne strategie dotyczące przesuwania kodu weryfikacyjnego z kontrolerów oraz do klas modeli.

> [!div class="step-by-step"]
> [Poprzednie](displaying-a-table-of-database-data-cs.md)
> [dalej](validating-with-the-idataerrorinfo-interface-cs.md)

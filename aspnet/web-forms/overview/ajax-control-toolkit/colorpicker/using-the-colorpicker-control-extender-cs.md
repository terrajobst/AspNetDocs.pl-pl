---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
title: Za pomocą rozszerzenia formantu ColorPicker (C#) | Microsoft Docs
author: microsoft
description: ColorPicker to rozszerzenie AJAX ASP.NET, które zapewnia funkcję wybierania koloru po stronie klienta z interfejsem użytkownika w kontrolce podręczny. Może być dołączony do dowolnego ASP.NET...
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 0d86a1e7-a910-4ab2-b85c-7a9ea6906c39
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: ac510ab353878038c1c7a103bfbf6d32fb1b2686
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614045"
---
# <a name="using-the-colorpicker-control-extender-c"></a>Korzystanie z rozszerzenia formantu ColorPicker (C#)

przez [firmę Microsoft](https://github.com/microsoft)

> ColorPicker to rozszerzenie AJAX ASP.NET, które zapewnia funkcję wybierania koloru po stronie klienta z interfejsem użytkownika w kontrolce podręczny. Może być dołączany do dowolnej kontrolki TextBox ASP.NET. Go.

Celem tego samouczka jest wyjaśnienie, jak można użyć rozszerzenia kontroli składnika ColorPicker zestawu narzędzi AJAX Control Toolkit. Rozszerzenie formantu ColorPicker wyświetla okno dialogowe, które umożliwia wybranie koloru. Składnik ColorPicker jest przydatny, gdy chcesz udostępnić intuicyjny interfejs użytkownika, aby użytkownik mógł wybrać kolor.

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a>Rozszerzanie formantu TextBox za pomocą rozszerzenia formantu ColorPicker

Załóżmy na przykład, że chcesz utworzyć witrynę sieci Web, która umożliwia odwiedzającym tworzenie dostosowanych kart firmowych. Odwiedzający mogą wprowadzić tekst dla karty biznesowej i wybrać kolor. Strona ASP.NET na liście 1 zawiera dwa kontrolki TextBox o nazwach txtCardText i txtCardColor. Po przesłaniu formularza wybrane wartości są wyświetlane (patrz rysunek 1).

[![prosty formularz do tworzenia karty biznesowej](using-the-colorpicker-control-extender-cs/_static/image1.jpg)](using-the-colorpicker-control-extender-cs/_static/image1.png)

**Ilustracja 01**: prosty formularz do tworzenia karty biznesowej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-colorpicker-control-extender-cs/_static/image2.png))

**Lista 1-karta. aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample1.aspx)]

Formularz z listą 1 działa, ale nie zapewnia doskonałego środowiska użytkownika. Użytkownik musi wpisać kolor do pola tekstowego. Jeśli użytkownik chce mieć wyspecjalizowany kolor — na przykład tylko prawy odcień PEA zielony — a następnie użytkownik musi ustalić kod koloru HTML bez pomocy.

Można użyć rozszerzenia formantu ColorPicker do utworzenia lepszego środowiska użytkownika. Składnik ColorPicker wyświetla okno dialogowe koloru po przeniesieniu fokusu do kontrolki TextBox (patrz rysunek 2).

[![rozszerzenia formantu ColorPicker](using-the-colorpicker-control-extender-cs/_static/image2.jpg)](using-the-colorpicker-control-extender-cs/_static/image3.png)

**Ilustracja 02**: rozszerzenie kontrolki ColorPicker ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-colorpicker-control-extender-cs/_static/image4.png))

Należy wykonać dwa kroki, aby użyć rozszerzenia formantu ColorPicker z formularzem na liście 1:

1. Dodawanie kontrolki ScriptManager do strony
2. Dodaj rozszerzenie kontrolki ColorPicker do strony

Aby można było używać składnika ColorPicker, należy dodać element ScriptManager do strony. Dobrym miejscem na dodanie elementu ScriptManager jest prawo poniżej otwierającego&gt; tagu &lt;po stronie serwera. Możesz przeciągnąć element ScriptManager na stronę z przybornika (element ScriptManager znajduje się na karcie rozszerzenia AJAX). Alternatywnie można wpisać następujący tag do widoku źródła poniżej otwierającego znacznika formularza po stronie serwera:

&lt;ASP: ScriptManager ID = "ScriptManager1" runat = "Server"/&gt;

Najprostszym sposobem dodania rozszerzenia formantu ColorPicker do strony jest widok projektu. Jeśli umieścisz wskaźnik myszy nad polem tekstowym txtCardColor, zostanie wyświetlona opcja inteligentnego zadania, która umożliwia dodanie rozszerzenia (patrz rysunek 3). W przypadku wybrania tej opcji zostanie wyświetlony Kreator rozszerzenia (zobacz rysunek 4).

[![dodawania rozszerzenia](using-the-colorpicker-control-extender-cs/_static/image3.jpg)](using-the-colorpicker-control-extender-cs/_static/image5.png)

**Ilustracja 03**: Dodawanie rozszerzenia ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-colorpicker-control-extender-cs/_static/image6.png))

[![Wybieranie rozszerzenia kontrolki za pomocą Kreatora rozszerzania](using-the-colorpicker-control-extender-cs/_static/image4.jpg)](using-the-colorpicker-control-extender-cs/_static/image7.png)

**Rysunek 04**: wybór rozszerzenia kontrolki za pomocą Kreatora rozszerzania ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-the-colorpicker-control-extender-cs/_static/image8.png))

Można wybrać rozszerzenie ColorPicker, aby rozszerzać pole tekstowe txtCardColor z przedłużaczem ColorPicker. Kliknij przycisk OK, aby zamknąć okno dialogowe.

Po wprowadzeniu tych zmian Źródło strony wygląda jak lista 2.

Lista 2 — Tworzenie karty. aspx (z ColorPicker)

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample2.aspx)]

Zauważ, że strona zawiera teraz kontrolkę ColorPickerExtender, która pojawia się bezpośrednio pod kontrolką TextBox txtCardColor. Formant ColorPickerExtender rozszerza formant txtCardColor tak, aby wyświetlał okno dialogowe selektora kolorów.

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a>Używanie przycisku do uruchamiania okna dialogowego selektora kolorów

Rozszerzenie ColorPicker obsługuje następujące właściwości:

- PopupButtonId — identyfikator przycisku na stronie, który powoduje wyświetlenie okna dialogowego selektora kolorów.
- PopupPosition — pozycja względem kontrolki Target okna dialogowego selektora kolorów. Możliwe wartości to bezwzględne, Center, BottomLeft, BottomRight, TopLeft, TopRight, Right i Left (wartość domyślna to BottomLeft).
- SampleControlId — identyfikator formantu, który wyświetla wybrany kolor.
- SelectedColor — początkowy kolor wybrany przez składnik ColorPicker.

Za pomocą tych właściwości można dostosować sposób wyświetlania okna dialogowego selektora kolorów oraz sposobu wyświetlania zaznaczonego koloru. Na stronie z listą 3 pokazano, jak można użyć kilku z tych właściwości.

**Lista 3-CreateCardButton. aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample3.aspx)]

Strona na liście 3 zawiera przycisk Wybierz kolor (patrz rysunek 5). Po kliknięciu tego przycisku zostanie wyświetlone okno dialogowe selektora kolorów powyżej pola tekstowego. W przypadku wybrania koloru z okna dialogowego wybrany kolor zostanie wyświetlony jako kolor tła kontrolki etykiety lblSample.

Właściwość ColorPicker PopupButtonID służy do kojarzenia przycisku Wybierz kolor z przedłużaczem ColorPicker. Gdy podasz wartość właściwości PopupButtonID, okno dialogowe selektora kolorów nie pojawia się już, gdy formant docelowy ma fokus. Aby wyświetlić okno dialogowe, należy kliknąć przycisk.

Właściwość SampleControlID służy do kojarzenia kontrolki wyświetlającej wybrany kolor z elementem ColorPicker. Składnik ColorPicker zmienia kolor tła kontrolki na aktualnie wybrany kolor.

[![wyświetlania okna dialogowego selektora kolorów z przyciskiem](using-the-colorpicker-control-extender-cs/_static/image5.jpg)](using-the-colorpicker-control-extender-cs/_static/image9.png)

**Ilustracja 05**: wyświetlanie okna dialogowego selektora kolorów z przyciskiem ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-colorpicker-control-extender-cs/_static/image10.png))

## <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono sposób użycia rozszerzenia formantu ColorPicker do wyświetlania okna dialogowego selektora kolorów menu podręcznego. Najpierw sprawdziłem, jak można wyświetlić okno dialogowe, gdy fokus jest przenoszony do kontrolki TextBox. Następnie pokazano, jak utworzyć przycisk, który wyświetla okno dialogowe selektora kolorów po kliknięciu przycisku.

> [!div class="step-by-step"]
> [Dalej](using-the-colorpicker-control-extender-vb.md)

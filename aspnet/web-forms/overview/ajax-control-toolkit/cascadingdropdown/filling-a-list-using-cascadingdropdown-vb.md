---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
title: Wypełnianie listy przy użyciu kontrolki CascadingDropDown (VB) | Microsoft Docs
author: wenz
description: Kontrolka kontrolki CascadingDropDown w zestawie narzędzi AJAX Control rozszerza kontrolkę DropDownList, tak aby zmiany w jednej DropDownList ładowały skojarzone wartości w anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 5236695e-5c70-4887-baee-0bfb0afb3448
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 8dd9ef8a4bdf705ba4451b7fd240e4de8618221c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599596"
---
# <a name="filling-a-list-using-cascadingdropdown-vb"></a>Wypełnianie listy przy użyciu kontrolki CascadingDropDown (VB)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)

> Kontrolka kontrolki CascadingDropDown w zestawie narzędzi AJAX Control rozszerza kontrolkę DropDownList, tak aby zmiany w jednej DropDownList ładowały skojarzone wartości w innym DropDownList. (Na przykład jedna lista zawiera listę stanów USA, a następna lista jest wypełniana głównymi miastami tego stanu). Pierwszym wyzwaniem do rozwiązania jest rzeczywiste wypełnienie listy rozwijanej przy użyciu tej kontrolki.

## <a name="overview"></a>Omówienie

Kontrolka kontrolki CascadingDropDown w zestawie narzędzi AJAX Control rozszerza kontrolkę DropDownList, tak aby zmiany w jednej DropDownList ładowały skojarzone wartości w innym DropDownList. (Na przykład jedna lista zawiera listę stanów USA, a następna lista jest wypełniana głównymi miastami tego stanu). Pierwszym wyzwaniem do rozwiązania jest rzeczywiste wypełnienie listy rozwijanej przy użyciu tej kontrolki.

## <a name="steps"></a>Kroki

Aby aktywować funkcje ASP.NET AJAX i zestawu narzędzi sterowania, formant `ScriptManager` musi być umieszczony w dowolnym miejscu na stronie (ale w obrębie elementu `<form>`):

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample1.aspx)]

Następnie wymagany jest formant DropDownList:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample2.aspx)]

Dla tej listy zostanie dodany kontrolki CascadingDropDown. Wyśle żądanie asynchroniczne do usługi sieci Web, która następnie zwróci listę wpisów do wyświetlenia na liście. Aby to działało, należy ustawić następujące atrybuty kontrolki CascadingDropDown:

- `ServicePath`: adres URL usługi sieci Web dostarczającej wpisy listy
- `ServiceMethod`: Metoda sieci Web dostarczająca wpisy listy
- `TargetControlID`: Identyfikator listy rozwijanej
- `Category`: informacje o kategorii, które są przesyłane do metody sieci Web w przypadku wywołania
- `PromptText`: tekst wyświetlany podczas asynchronicznego ładowania danych listy z serwera

Poniżej znajduje się znacznik elementu `CascadingDropDown`. Jedyną różnicą między C# i VB jest nazwa skojarzonej usługi sieci Web:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample3.aspx)]

Kod JavaScript pochodzący z rozszerzenia `CascadingDropDown` rozszerza wywołanie metody usługi sieci Web z następującym podpisem:

[!code-vb[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample4.vb)]

Dlatego ważne jest, aby Metoda zwracała tablicę typu `CascadingDropDownNameValue` (zdefiniowana przez zestaw narzędzi ASP.NET AJAX Control Toolkit). W konstruktorze `CascadingDropDownNameValue` najpierw tekst wpisu listy, a następnie należy podać jego wartość, podobnie jak w przypadku `<option value="VALUE">NAME</option>` w kodzie HTML. Oto kilka przykładowych danych:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample5.aspx)]

Załadowanie strony w przeglądarce spowoduje wyzwolenie listy dla trzech dostawców.

[![lista jest wypełniana automatycznie](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)

Lista jest wypełniana automatycznie ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](using-auto-postback-with-cascadingdropdown-cs.md)
> [dalej](using-cascadingdropdown-with-a-database-vb.md)

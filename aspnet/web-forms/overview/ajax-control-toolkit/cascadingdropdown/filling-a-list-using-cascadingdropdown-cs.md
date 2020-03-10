---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
title: Wypełnianie listy przy użyciuC#kontrolki CascadingDropDown () | Microsoft Docs
author: wenz
description: Kontrolka kontrolki CascadingDropDown w zestawie narzędzi AJAX Control rozszerza kontrolkę DropDownList, tak aby zmiany w jednej DropDownList ładowały skojarzone wartości w anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: f949aafa-fe57-43b0-b722-f0dd33a900be
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: b5e9874fb5b6d3e55c8af5b85d12bf1ffacc116b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614213"
---
# <a name="filling-a-list-using-cascadingdropdown-c"></a>Wypełnianie listy przy użyciu kontrolki CascadingDropDown (C#)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)

> Kontrolka kontrolki CascadingDropDown w zestawie narzędzi AJAX Control rozszerza kontrolkę DropDownList, tak aby zmiany w jednej DropDownList ładowały skojarzone wartości w innym DropDownList. (Na przykład jedna lista zawiera listę stanów USA, a następna lista jest wypełniana głównymi miastami tego stanu). Pierwszym wyzwaniem do rozwiązania jest rzeczywiste wypełnienie listy rozwijanej przy użyciu tej kontrolki.

## <a name="overview"></a>Omówienie

Kontrolka kontrolki CascadingDropDown w zestawie narzędzi AJAX Control rozszerza kontrolkę DropDownList, tak aby zmiany w jednej DropDownList ładowały skojarzone wartości w innym DropDownList. (Na przykład jedna lista zawiera listę stanów USA, a następna lista jest wypełniana głównymi miastami tego stanu). Pierwszym wyzwaniem do rozwiązania jest rzeczywiste wypełnienie listy rozwijanej przy użyciu tej kontrolki.

## <a name="steps"></a>Kroki

Aby aktywować funkcje ASP.NET AJAX i zestawu narzędzi sterowania, formant `ScriptManager` musi być umieszczony w dowolnym miejscu na stronie (ale w obrębie elementu `<form>`):

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample1.aspx)]

Następnie wymagany jest formant DropDownList:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample2.aspx)]

Dla tej listy zostanie dodany kontrolki CascadingDropDown. Wyśle żądanie asynchroniczne do usługi sieci Web, która następnie zwróci listę wpisów do wyświetlenia na liście. Aby to działało, należy ustawić następujące atrybuty kontrolki CascadingDropDown:

- `ServicePath`: adres URL usługi sieci Web dostarczającej wpisy listy
- `ServiceMethod`: Metoda sieci Web dostarczająca wpisy listy
- `TargetControlID`: Identyfikator listy rozwijanej
- `Category`: informacje o kategorii, które są przesyłane do metody sieci Web w przypadku wywołania
- `PromptText`: tekst wyświetlany podczas asynchronicznego ładowania danych listy z serwera

Poniżej znajduje się znacznik elementu `CascadingDropDown`. Jedyną różnicą między C# i VB jest nazwa skojarzonej usługi sieci Web:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample3.aspx)]

Kod JavaScript pochodzący z rozszerzenia `CascadingDropDown` rozszerza wywołanie metody usługi sieci Web z następującym podpisem:

[!code-csharp[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample4.cs)]

Dlatego ważne jest, aby Metoda zwracała tablicę typu `CascadingDropDownNameValue` (zdefiniowana przez zestaw narzędzi ASP.NET AJAX Control Toolkit). W konstruktorze `CascadingDropDownNameValue` najpierw tekst wpisu listy, a następnie należy podać jego wartość, podobnie jak w przypadku `<option value="VALUE">NAME</option>` w kodzie HTML. Oto kilka przykładowych danych:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample5.aspx)]

Załadowanie strony w przeglądarce spowoduje wyzwolenie listy dla trzech dostawców.

[![lista jest wypełniana automatycznie](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)

Lista jest wypełniana automatycznie ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Dalej](using-cascadingdropdown-with-a-database-cs.md)

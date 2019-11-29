---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
title: Ustawianie wpisów listy z kontrolki CascadingDropDown (VB) | Microsoft Docs
author: wenz
description: Kontrolka kontrolki CascadingDropDown w zestawie narzędzi AJAX Control rozszerza kontrolkę DropDownList, tak aby zmiany w jednej DropDownList ładowały skojarzone wartości w anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: ec61ced7-bbca-4bdd-aa3b-80878f295181
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 58d675993777f9dcbe0ce1890a60046c91ee8907
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599546"
---
# <a name="presetting-list-entries-with-cascadingdropdown-vb"></a>Wstępne ustawianie pozycji listy przy użyciu kontrolki CascadingDropDown (VB)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)

> Kontrolka kontrolki CascadingDropDown w zestawie narzędzi AJAX Control rozszerza kontrolkę DropDownList, tak aby zmiany w jednej DropDownList ładowały skojarzone wartości w innym DropDownList. W przypadku niewielkiej ilości kodu jest możliwe, że element listy jest wybierany po załadowaniu danych dynamicznie.

## <a name="overview"></a>Omówienie

Kontrolka kontrolki CascadingDropDown w zestawie narzędzi AJAX Control rozszerza kontrolkę DropDownList, tak aby zmiany w jednej DropDownList ładowały skojarzone wartości w innym DropDownList. (Na przykład jedna lista zawiera listę stanów USA, a następna lista jest wypełniana głównymi miastami tego stanu). W przypadku niewielkiej ilości kodu jest możliwe, że element listy jest wybierany po załadowaniu danych dynamicznie.

## <a name="steps"></a>Kroki

Aby aktywować funkcje ASP.NET AJAX i zestawu narzędzi sterowania, formant `ScriptManager` musi być umieszczony w dowolnym miejscu na stronie (ale w obrębie elementu `<form>`):

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample1.aspx)]

Następnie wymagany jest formant DropDownList:

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample2.aspx)]

Dla tej listy dodano rozszerzenie kontrolki CascadingDropDown, podając adres URL i informacje o metodzie usługi sieci Web:

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample3.aspx)]

Rozszerzenie kontrolki CascadingDropDown następnie asynchronicznie wywołuje usługę sieci Web przy użyciu następującej sygnatury metody:

[!code-vb[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample4.vb)]

Metoda zwraca tablicę typu kontrolki CascadingDropDown wartość. Konstruktor typu oczekuje najpierw podpisu wpisu listy, a następnie wartość (HTML `value` atrybutu). Jeśli trzeci argument ma wartość true, element list zostanie automatycznie wybrany w przeglądarce.

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample5.aspx)]

Załadowanie strony w przeglądarce spowoduje wypełnienie listy rozwijanej trzema dostawcami, a drugi jest wybierany w pierwszej postaci.

[![lista jest wypełniana i wybierana automatycznie](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)

Lista jest wypełniana i wybierana automatycznie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](using-cascadingdropdown-with-a-database-vb.md)
> [dalej](using-auto-postback-with-cascadingdropdown-vb.md)

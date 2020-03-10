---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
title: Korzystanie z funkcji autoogłaszania zwrotnego z kontrolki CascadingDropDown (C#) | Microsoft Docs
author: wenz
description: Kontrolka kontrolki CascadingDropDown w zestawie narzędzi AJAX Control rozszerza kontrolkę DropDownList, tak aby zmiany w jednej DropDownList ładowały skojarzone wartości w anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 6755d8d9-14be-4a1d-86e5-1a6110f3dea8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: 8bccd716814e7de544798010cecbc148ec50b5cd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78535953"
---
# <a name="using-auto-postback-with-cascadingdropdown-c"></a>Używanie automatycznego ogłaszania zwrotnego za pomocą kontrolki CascadingDropDown (C#)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3CS.pdf)

> Kontrolka kontrolki CascadingDropDown w zestawie narzędzi AJAX Control rozszerza kontrolkę DropDownList, tak aby zmiany w jednej DropDownList ładowały skojarzone wartości w innym DropDownList. Jednak w przypadku korzystania z formantu kontrolki CascadingDropDown, ASP. Funkcja autoogłaszania DropDownList w sieci nie działa, ponieważ asynchroniczne ładowanie danych do listy generuje (niepotrzebne) ogłaszanie zwrotne. W przypadku niektórych kodów JavaScript ten efekt można uniknąć.

## <a name="overview"></a>Omówienie

Kontrolka kontrolki CascadingDropDown w zestawie narzędzi AJAX Control rozszerza kontrolkę DropDownList, tak aby zmiany w jednej DropDownList ładowały skojarzone wartości w innym DropDownList. (Na przykład jedna lista zawiera listę stanów USA, a następna lista jest wypełniana głównymi miastami tego stanu). Jednak w przypadku korzystania z formantu kontrolki CascadingDropDown, ASP. Funkcja autoogłaszania DropDownList w sieci nie działa, ponieważ asynchroniczne ładowanie danych do listy generuje (niepotrzebne) ogłaszanie zwrotne. W przypadku niektórych kodów JavaScript ten efekt można uniknąć.

## <a name="steps"></a>Kroki

Aby uaktywnić funkcje ASP.NET AJAX i zestaw narzędzi sterowania, formant `ScriptManager` musi być umieszczony w dowolnym miejscu na stronie (ale w &lt;`form`&gt; elementu):

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample1.aspx)]

Następnie wymagany jest formant DropDownList:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample2.aspx)]

Dla tej listy dodano rozszerzenie kontrolki CascadingDropDown, podając adres URL i informacje o metodzie usługi sieci Web:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample3.aspx)]

Rozszerzenie kontrolki CascadingDropDown następnie asynchronicznie wywołuje usługę sieci Web przy użyciu następującej sygnatury metody:

[!code-csharp[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample4.cs)]

Metoda zwraca tablicę typu kontrolki CascadingDropDown wartość. Konstruktor typu oczekuje najpierw podpisu wpisu listy, a następnie wartość (HTML `value` atrybutu).

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample5.aspx)]

Załadowanie strony w przeglądarce spowoduje wypełnienie listy rozwijanej trzema dostawcami, a drugi jest wybierany w pierwszej postaci. Ponadto ASP.NET definiuje metodę `__doPostBack()` JavaScript. Po załadowaniu strony to wywołanie języka JavaScript jest dodawane do listy rozwijanej, ale tylko wtedy, gdy znajdują się w niej elementy. Jeśli na liście nie ma elementów, zestaw narzędzi do sterowania jest obecnie ładowany, więc kod JavaScript używa limitu czasu i próbuje ponownie za pół sekundy.

[!code-html[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample6.html)]

W ten sposób ogłaszanie zwrotne jest wykonywane tylko wtedy, gdy na liście znajdują się elementy, a użytkownik wybierze wpis.

[![wybranie elementu listy spowoduje odświeżenie](using-auto-postback-with-cascadingdropdown-cs/_static/image2.png)](using-auto-postback-with-cascadingdropdown-cs/_static/image1.png)

Wybranie elementu listy powoduje ogłoszenie zwrotne ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-auto-postback-with-cascadingdropdown-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](presetting-list-entries-with-cascadingdropdown-cs.md)
> [dalej](filling-a-list-using-cascadingdropdown-vb.md)

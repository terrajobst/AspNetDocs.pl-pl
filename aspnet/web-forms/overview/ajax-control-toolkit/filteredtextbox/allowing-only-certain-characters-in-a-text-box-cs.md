---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
title: Zezwalanie tylko na niektóre znaki w polu tekstowym (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolek weryfikacji platformy ASP.NET można upewnić się, że tylko na niektóre znaki są dozwolone w danych wejściowych użytkownika. Jednak to nadal nie uniemożliwia użytkownikom wpisywanie nieprawidłowy...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: fd2a1c52-d717-44af-8a61-67c8279bb26e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
msc.type: authoredcontent
ms.openlocfilehash: d8a1e792c9cd854591fc434f28afe98e4d91dfbe
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57068012"
---
<a name="allowing-only-certain-characters-in-a-text-box-c"></a>Zezwalanie tylko na niektóre znaki w polu tekstowym (C#)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)

> Kontrolek weryfikacji platformy ASP.NET można upewnić się, że tylko na niektóre znaki są dozwolone w danych wejściowych użytkownika. Jednak to nadal nie uniemożliwia użytkownikom wpisywanie nieprawidłowe znaki i podjęcie próby można przesłać formularza.


## <a name="overview"></a>Omówienie

Kontrolek weryfikacji platformy ASP.NET można upewnić się, że tylko na niektóre znaki są dozwolone w danych wejściowych użytkownika. Jednak to nadal nie uniemożliwia użytkownikom wpisywanie nieprawidłowe znaki i podjęcie próby można przesłać formularza.

## <a name="steps"></a>Kroki

ASP.NET AJAX Control Toolkit zawiera `FilteredTextBox` kontrolki, która rozszerza pola tekstowego. Po uaktywnieniu tylko w określonym zestawie znaków mogą być wprowadzane w polu.

Aby to zrobić, najpierw należy w zwykły sposób ASP.NET AJAX `ScriptManager` która ładuje bibliotek JavaScript, które są również używane przez program ASP.NET AJAX Control Toolkit:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample1.aspx)]

W efekcie potrzebujemy pola tekstowego:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample2.aspx)]

Na koniec `FilteredTextBoxExtender` kontrolka zajmuje się ograniczenie znaków, użytkownik może wpisać. Najpierw ustaw `TargetControlID` atrybutu `ID` z `TextBox` kontroli. Następnie wybierz jedną z dostępnych `FilterType` wartości:

- `Custom` Domyślnie; Musisz podać listę prawidłowe znaki
- `LowercaseLetters` małe litery
- `Numbers` tylko cyfry
- `UppercaseLetters` tylko wielkie litery

Jeśli `Custom FilterType` jest używany, `ValidChars` właściwość musi być ustawione i podać listę znaków, które mogą być wpisana. Przy okazji: Jeśli użytkownik próbuje wkleić tekst w polu tekstowym, zostaną usunięte wszystkie nieprawidłowe znaki.

Oto znaczniki dla `FilteredTextBoxExtender` formant, który dopuszcza tylko cyfr (coś, który także byłby możliwego `FilterType="Numbers"`):

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample3.aspx)]

Uruchomić stronę i spróbuj wprowadzić literą, jeśli włączona JavaScript, nie będzie on działał; cyfry są jednak wyświetlane na stronie. Jednak należy pamiętać, że ochrona `FilteredTextBox` zapewnia nie jest dowód punktor: Jeśli JavaScript jest włączony, wszelkie dane mogą być wprowadzane w polu tekstowym, więc trzeba użyć oznacza, że dodatkowe sprawdzenie poprawności, czyli ASP. Formanty sprawdzania poprawności w sieci.


[![Mogą być wprowadzane tylko cyfry](allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)

Mogą być wprowadzane tylko cyfry ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](allowing-only-certain-characters-in-a-text-box-vb.md)

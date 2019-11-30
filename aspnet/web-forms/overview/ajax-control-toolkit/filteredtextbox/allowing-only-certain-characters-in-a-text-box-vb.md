---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
title: Zezwalanie tylko na niektóre znaki w polu tekstowym (VB) | Microsoft Docs
author: wenz
description: Kontrolki walidacji ASP.NET mogą zapewnić, że w danych wejściowych użytkownika dozwolone są tylko pewne znaki. Jednak nadal nie jest możliwe wpisywanie przez użytkowników nieprawidłowych znaków...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 33af23f1-4016-4740-8fb2-37d1773452cd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
msc.type: authoredcontent
ms.openlocfilehash: 895708ebecc30c5f35e6ecd0349604bb777cbd93
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74573952"
---
# <a name="allowing-only-certain-characters-in-a-text-box-vb"></a>Zezwalanie tylko na niektóre znaki w polu tekstowym (VB)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)

> Kontrolki walidacji ASP.NET mogą zapewnić, że w danych wejściowych użytkownika dozwolone są tylko pewne znaki. Mimo to nadal nie jest możliwe wpisywanie przez użytkowników nieprawidłowych znaków i próba przesłania formularza.

## <a name="overview"></a>Omówienie

Kontrolki walidacji ASP.NET mogą zapewnić, że w danych wejściowych użytkownika dozwolone są tylko pewne znaki. Mimo to nadal nie jest możliwe wpisywanie przez użytkowników nieprawidłowych znaków i próba przesłania formularza.

## <a name="steps"></a>Kroki

Zestaw narzędzi ASP.NET AJAX Control Toolkit zawiera kontrolkę `FilteredTextBox`, która rozszerza pole tekstowe. Po aktywowaniu tego pola może być wprowadzony tylko określony zestaw znaków.

Aby to działało, najpierw musimy być zwykle ASP.NET AJAX `ScriptManager`, która ładuje biblioteki JavaScript, które są również używane przez zestaw narzędzi ASP.NET AJAX Control Toolkit:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample1.aspx)]

Następnie potrzebujemy pola tekstowego:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample2.aspx)]

Na koniec kontrola `FilteredTextBoxExtender` ma na celu ograniczenie znaków, które użytkownik może wpisać. Najpierw ustaw atrybut `TargetControlID` na `ID` kontrolki `TextBox`. Następnie wybierz jedną z dostępnych wartości `FilterType`:

- `Custom` domyślne; musisz podać listę prawidłowych znaków
- tylko `LowercaseLetters` małe litery
- tylko cyfry `Numbers`
- tylko `UppercaseLetters` wielkie litery

Jeśli `Custom FilterType` jest używany, właściwość `ValidChars` musi być ustawiona i zawierać listę znaków, które mogą zostać wpisane. Według sposobu: Jeśli spróbujesz wkleić tekst do pola tekstowego, wszystkie nieprawidłowe znaki są usuwane.

Poniżej znajduje się znacznik kontrolki `FilteredTextBoxExtender`, który zezwala tylko na cyfry (co również było możliwe w przypadku `FilterType="Numbers"`):

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample3.aspx)]

Uruchom stronę i spróbuj wprowadzić literę, jeśli język JavaScript jest włączony, nie będzie działać. cyfry są jednak wyświetlane na stronie. Należy jednak pamiętać, że `FilteredTextBox` ochrony nie jest punktorem-testowym: Jeśli JavaScript jest włączony, wszystkie dane mogą być wprowadzane w polu tekstowym, więc należy użyć dodatkowej weryfikacji, np. ASP. Kontrolki walidacji netto.

[można wprowadzać tylko ![cyfry](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)

Można wprowadzić tylko cyfry ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Ubiegł](allowing-only-certain-characters-in-a-text-box-cs.md)

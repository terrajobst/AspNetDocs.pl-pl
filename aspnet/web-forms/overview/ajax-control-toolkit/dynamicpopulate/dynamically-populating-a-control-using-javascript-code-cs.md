---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
title: Dynamiczne wypełnianie kontrolki przy użyciuC#kodu JavaScript () | Microsoft Docs
author: wenz
description: Kontrolka używanie dynamicpopulate w zestawie narzędzi ASP.NET AJAX Control wywołuje usługę sieci Web (lub metodę strony) i wypełnia wartość wyniki do kontrolki docelowej na t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: cc4c2def-e88c-4456-ae8b-a6ae0ff8cc2d
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 24dc358427dec3ffcba16d00041c9a2db657e7e2
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599263"
---
# <a name="dynamically-populating-a-control-using-javascript-code-c"></a>Dynamiczne wypełnianie kontrolki przy użyciu kodu JavaScript (C#)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)

> Kontrolka używanie dynamicpopulate w zestawie narzędzi ASP.NET AJAX Control wywołuje usługę sieci Web (lub metodę strony) i wypełnia wartość wyniki do kontrolki docelowej na stronie bez odświeżania strony. Możliwe jest również wyzwolenie populacji przy użyciu niestandardowego kodu JavaScript po stronie klienta.

## <a name="overview"></a>Omówienie

Formant `DynamicPopulate` w zestawie narzędzi ASP.NET AJAX Control wywołuje usługę sieci Web (lub metodę strony) i wypełnia wartość wyniki do kontrolki docelowej na stronie bez odświeżania strony. Możliwe jest również wyzwolenie populacji przy użyciu niestandardowego kodu JavaScript po stronie klienta.

## <a name="steps"></a>Kroki

Najpierw potrzebna jest usługa sieci Web ASP.NET, która implementuje metodę, która ma zostać wywołana przez formant `DynamicPopulateExtender`. Usługa sieci Web implementuje metodę `getDate()`, która oczekuje jednego argumentu typu ciąg o nazwie `contextKey`, ponieważ kontrolka `DynamicPopulate` wysyła jedną część informacji o kontekście do każdego wywołania usługi sieci Web. Oto kod (plik `DynamicPopulate.cs.asmx`), który pobiera bieżącą datę w jednym z trzech formatów:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample1.aspx)]

W następnym kroku Utwórz nową witrynę ASP.NET i Rozpocznij pracę z kontrolką ScriptManager ASP.NET AJAX:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample2.aspx)]

Następnie Dodaj kontrolkę etykieta (na przykład za pomocą kontrolki HTML o tej samej nazwie lub kontrolki sieci Web `<asp:Label />`), która będzie później wyświetlać wynik wywołania usługi sieci Web.

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample3.aspx)]

Następnie Uwzględnij formant `DynamicPopulateExtender` i podaj informacje o usłudze sieci Web, kontrolce docelowej, ale nie nazwę kontrolki, która wyzwala populację, którą zostanie później przeprowadzona, przy użyciu niestandardowego języka JavaScript.

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample4.aspx)]

Teraz ze składnikiem JavaScript. Funkcja `$find()` zdefiniowana przez ASP.NET AJAX Library zwraca odwołanie do obiektów po stronie serwera zestawu narzędzi ASP.NET AJAX Control Toolkit, takich jak `DynamicPopulateExtender`. W bieżącym pliku `$find("dpe")` zwraca odwołanie do kontrolki `DynamicPopulateExtender` na stronie. Uwidacznia metodę o nazwie `populate()`, która wyzwala proces wypełniania dynamicznego. Metoda `populate()` wymaga jednego argumentu: klucza kontekstu, który będzie używany jako argument dla metody `getDate()` sieci Web. Na przykład `$find("dpe").populate("format1")` zapełnił etykietę bieżącą datą w formacie dnia miesiąca.

Aby zwiększyć elastyczność przykładu, użytkownik może teraz wybrać kilka formatów daty. Dla każdej z nich zostanie wyświetlony przycisk radiowy. Gdy użytkownik kliknie przycisk radiowy, kod JavaScript dynamicznie wypełnia etykietę wybranym formatem daty. Poniżej znajdują się następujące przyciski radiowe:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample5.aspx)]

Należy zauważyć, że w kontekście przycisku radiowego wyrażenie JavaScript `this.value` odnosi się do wartości bieżącego przycisku, co ma na celu dokładnie te same informacje, z którymi może współdziałać `getDate()` Metoda.

[![kliknięcie przycisku pobiera datę z serwera w określonym formacie](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)

Kliknięcie przycisku pobiera datę z serwera w określonym formacie ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](dynamically-populating-a-control-cs.md)
> [dalej](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)

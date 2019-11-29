---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
title: Dynamiczne wypełnianie kontrolki (C#) | Microsoft Docs
author: wenz
description: Kontrolka używanie dynamicpopulate w zestawie narzędzi ASP.NET AJAX Control wywołuje usługę sieci Web (lub metodę strony) i wypełnia wartość wyniki do kontrolki docelowej na t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e1fec43e-1daf-49d2-b0c7-7f1b930455cc
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 24f88e44e0f878127314774d4e8846f80133413e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599299"
---
# <a name="dynamically-populating-a-control-c"></a>Dynamiczne wypełnianie kontrolki (C#)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0CS.pdf)

> Kontrolka używanie dynamicpopulate w zestawie narzędzi ASP.NET AJAX Control wywołuje usługę sieci Web (lub metodę strony) i wypełnia wartość wyniki do kontrolki docelowej na stronie bez odświeżania strony.

## <a name="overview"></a>Omówienie

Formant `DynamicPopulate` w zestawie narzędzi ASP.NET AJAX Control wywołuje usługę sieci Web (lub metodę strony) i wypełnia wartość wyniki do kontrolki docelowej na stronie bez odświeżania strony. W tym samouczku pokazano, jak to skonfigurować.

## <a name="steps"></a>Kroki

Najpierw potrzebna jest usługa sieci Web ASP.NET, która implementuje metodę, która ma zostać wywołana przez `DynamicPopulate`. Klasa usługi sieci Web wymaga atrybutu `ScriptService`, który jest zdefiniowany w ramach `Microsoft.Web.Script.Services`; w przeciwnym razie ASP.NET AJAX nie może utworzyć serwera proxy JavaScript po stronie klienta dla usługi sieci Web, która z kolei jest wymagana przez `DynamicPopulate`.

Metoda sieci Web musi oczekiwać jednego argumentu typu String o nazwie `contextKey`, ponieważ kontrolka `DynamicPopulate` wysyła jedną część informacji o kontekście do każdego wywołania usługi sieci Web. Poniższa usługa sieci Web zwraca bieżącą datę w formacie reprezentowanym przez argument `contextKey`:

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample1.aspx)]

Usługa sieci Web jest następnie zapisywana jako `DynamicPopulate.cs.asmx`. Alternatywnie można zaimplementować metodę `getDate()` jako metodę strony na rzeczywistej stronie ASP.NET z kontrolką `DynamicPopulate`.

W następnym kroku Utwórz nowy plik ASP.NET. Jak zawsze, pierwszym krokiem jest uwzględnienie `ScriptManager` na bieżącej stronie, aby załadować bibliotekę AJAX ASP.NET i aby działała zestaw narzędzi sterowania:

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample2.aspx)]

Następnie Dodaj kontrolkę etykieta (na przykład przy użyciu kontrolki HTML o tej samej nazwie lub &lt;`asp:Label` /&gt; formant sieci Web), która będzie później wyświetlać wynik wywołania usługi sieci Web.

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample3.aspx)]

Przycisk HTML (jako kontrolka HTML, ponieważ nie wymaga ogłaszania zwrotnego na serwerze) będzie używany do wyzwalania populacji dynamicznej:

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample4.aspx)]

Na koniec potrzebujemy formantu `DynamicPopulateExtender`, aby obprowadzić działania. Następujące atrybuty zostaną ustawione (poza oczywistymi, `ID` i `runat`=`"server"`):

- `TargetControlID` miejsca, w którym ma zostać umieszczony wynik wywołania usługi sieci Web
- `ServicePath` ścieżkę do usługi sieci Web (Pomiń, jeśli chcesz użyć metody strony)
- `ServiceMethod` nazwę metody sieci Web lub metody strony
- `ContextKey` informacje kontekstu do wysłania do usługi sieci Web
- `PopulateTriggerControlID` element wyzwalający wywołanie usługi sieci Web
- `ClearContentsDuringUpdate`, czy element docelowy ma być opróżniany podczas wywołania usługi sieci Web

Jak widać, formant wymaga pewnych informacji, ale umieszczenie wszystkiego w miejscu jest bardzo proste. Poniżej znajduje się znacznik kontrolki `DynamicPopulateExtender` w bieżącym scenariuszu:

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample5.aspx)]

Uruchom stronę ASP.NET w przeglądarce i kliknij przycisk; Bieżąca data zostanie wyświetlona w formacie miesiąca dziennego.

[![kliknięcie przycisku pobiera datę z serwera](dynamically-populating-a-control-cs/_static/image2.png)](dynamically-populating-a-control-cs/_static/image1.png)

Kliknięcie przycisku pobiera datę z serwera ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](dynamically-populating-a-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](dynamically-populating-a-control-using-javascript-code-cs.md)

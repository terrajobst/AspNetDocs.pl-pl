---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-vb
title: Zwijanie i rozszerzanie panelu z JavaScript (VB) | Microsoft Docs
author: wenz
description: Kontrolka CollapsiblePanel w zestawie narzędzi ASP.NET AJAX Control rozszerza panel i udostępnia ją z możliwością zwijania jej zawartości i rozszerzania jej...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 298789b4-2964-49f5-a0a8-d4dbeb9ff2c2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: aa9779c65fb587193dbabde55cc6900283ce239d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78535848"
---
# <a name="collapsing-and-expanding-a-panel-from-javascript-vb"></a>Rozwijanie i zwijanie panelu z poziomu języka JavaScript (VB)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1VB.pdf)

> Kontrolka CollapsiblePanel w zestawie narzędzi ASP.NET AJAX Control rozszerza panel i udostępnia ją z możliwością zwijania jej zawartości i jej ponownego rozwinięcia. Te dwie akcje mogą być również wywoływane z niestandardowego kodu JavaScript.

## <a name="overview"></a>Omówienie

Kontrolka CollapsiblePanel w zestawie narzędzi ASP.NET AJAX Control rozszerza panel i udostępnia ją z możliwością zwijania jej zawartości i jej ponownego rozwinięcia. Te dwie akcje mogą być również wywoływane z niestandardowego kodu JavaScript.

## <a name="steps"></a>Kroki

Najpierw utwórz nową stronę ASP.NET i Uwzględnij `ScriptManager` w obrębie jednego `<form>` elementu. Spowoduje to załadowanie biblioteki AJAX ASP.NET, która jest wymagana przez zestaw narzędzi sterowania:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample1.aspx)]

Następnie utwórz panel z tekstem, tak aby można było zobaczyć efekt zwijania/rozwijania:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample2.aspx)]

Jak widać, panel odwołuje się do klasy CSS, która jest wyświetlana w tym miejscu (i w istocie definiuje kolor tła i szerokość panelu):

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample3.css)]

Kontrolka `CollapsiblePanelExtender` wymaga atrybutu `TargetControlID`, dzięki czemu zestaw narzędzi wie, który panel ma zostać zwinięty lub rozwinięty na żądanie:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample4.aspx)]

Niestety, rozszerzenie aktualnie nie uwidacznia określonego interfejsu API do zwijania lub rozszerzania panelu, ale niektóre nieudokumentowane metody. Po pierwsze, Dodaj do strony trzy przyciski HTML, które będą wyzwalać kod JavaScript po stronie klienta, aby zwinąć lub rozwinąć zawartość panelu:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample5.aspx)]

W kodzie JavaScript po stronie klienta (rozpoczętym z `<script type="text/javascript">`) Metoda `$find()` musi być używana w celu uzyskania dostępu do `CollapsiblePanelExtender`. `$find("cpe")` zwróci odwołanie do niego. Z tego miejsca poszczególne metody rozwiązują zadanie.

Metoda otwierająca (rozszerzająca) Panel jest nazywana `_doOpen()`; Poniższy kod implementuje funkcję `doOpen()` wywołana po kliknięciu pierwszego przycisku:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample6.js)]

Aby zamknąć lub zwinąć panel, należy wykonać metodę `_doClose()`. Gdy użytkownik kliknie drugi przycisk, zostanie wywołany następujący kod JavaScript:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample7.js)]

Trzeci przycisk przełącza stan panelu: od zwinięte do rozwinięte i na odwrót. `CollapsiblePanelExtender` uwidacznia metodę `toggle()`, która dokładnie to oznacza, że: odwraca stan panelu. Istnieje również inne podejście (używane wewnętrznie przez metodę `toggle()`): Metoda `get_Collapsed()` `CollapsiblePanelExtender()` informuje nas o tym, czy panel jest zwinięty, czy nie. W zależności od wartości zwracanej przez tę funkcję panel jest następnie rozwinięty (`_doOpen()` metodzie) lub zwinięty (`_doClose()`):

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample8.js)]

[![trzeci przycisk zmienia stan panelu: od zwinięte do rozwinięte i z powrotem](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image1.png)

Trzeci przycisk zmienia stan panelu: od zwiniętej do rozwiniętej i z powrotem ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Wstecz](collapsing-and-expanding-a-panel-from-javascript-cs.md)

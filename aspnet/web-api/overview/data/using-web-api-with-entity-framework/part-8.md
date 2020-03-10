---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: Wyświetl szczegóły elementu | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 3f48c5ad73ceb9a4873fbbb621b518553a498966
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557324"
---
# <a name="display-item-details"></a>Wyświetlanie szczegółów elementu

według [Jan Wasson](https://github.com/MikeWasson)

[Pobierz ukończony projekt](https://github.com/MikeWasson/BookService)

W tej sekcji dodasz możliwość wyświetlania szczegółów dla każdej książki. W aplikacji App. js Dodaj do modelu widoku następujący kod:

[!code-javascript[Main](part-8/samples/sample1.js)]

W widokach/Home/index. cshtml Dodaj element powiązania danych do linku szczegóły:

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

To wiąże procedurę obsługi kliknięcia dla &lt;elementu&gt; do funkcji `getBookDetail` w modelu widoku.

W tym samym pliku Zastąp następujące oznakowanie:

[!code-html[Main](part-8/samples/sample3.html)]

na kod:

[!code-html[Main](part-8/samples/sample4.html)]

Ten znacznik tworzy tabelę zawierającą dane powiązane z właściwościami `detail` widocznymi w modelu widoku.

Składnia "&lt;!--ko-&quot;&gt;umożliwia uwzględnienie powiązania odcinania poza elementem modelu DOM. W takim przypadku powiązanie `if` powoduje, że ta sekcja znaczników będzie wyświetlana tylko wtedy, gdy `details` ma wartość różną od null.

[!code-html[Main](part-8/samples/sample5.html)]

Teraz, jeśli uruchomisz aplikację i klikniesz jeden z &quot;szczegóły&quot; linki, aplikacja wyświetli Szczegóły książki.

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

> [!div class="step-by-step"]
> [Poprzednie](part-7.md)
> [dalej](part-9.md)

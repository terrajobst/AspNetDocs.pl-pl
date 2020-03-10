---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: Dodaj nowy element do bazy danych | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: 692269a2c11e529af78f24feca74bba704b5b54b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557289"
---
# <a name="add-a-new-item-to-the-database"></a>Dodawanie nowego elementu do bazy danych

według [Jan Wasson](https://github.com/MikeWasson)

[Pobierz ukończony projekt](https://github.com/MikeWasson/BookService)

W tej sekcji dodasz możliwość tworzenia nowej książki przez użytkowników. W aplikacji App. js Dodaj następujący kod do modelu widoku:

[!code-javascript[Main](part-9/samples/sample1.js)]

W index. cshtml Zastąp następujące znaczniki:

[!code-html[Main](part-9/samples/sample2.html)]

Tym:

[!code-html[Main](part-9/samples/sample3.html)]

Ten znacznik tworzy formularz do przesyłania nowego autora. Wartości dla listy rozwijanej autor są powiązane z `authors` widocznymi w modelu widoku. W przypadku innych danych wejściowych formularza wartości są powiązane z danymi właściwością `newBook` modelu widoku.

Procedura obsługi przesyłania w formularzu jest powiązana z funkcją `addBook`:

[!code-html[Main](part-9/samples/sample4.html)]

Funkcja `addBook` odczytuje bieżące wartości danych wejściowych formularza powiązanych z danymi, aby utworzyć obiekt JSON. Następnie zapisuje obiekt JSON w `/api/books`.

> [!div class="step-by-step"]
> [Poprzednie](part-8.md)
> [dalej](part-10.md)

---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-vb
title: Używanie używanie textboxwatermark w widoku FormView (VB) | Microsoft Docs
author: wenz
description: Kontrolka używanie textboxwatermark w zestawie narzędzi AJAX Control rozszerza pole tekstowe tak, aby tekst był wyświetlany w polu. Gdy użytkownik kliknie w polu i...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 41497361-7fba-4825-b36c-f58d79522a88
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-vb
msc.type: authoredcontent
ms.openlocfilehash: 1dd17ed57383680122c4ca613ca71f6682dec40e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78627107"
---
# <a name="using-textboxwatermark-in-a-formview-vb"></a>Używanie kontrolki TextBoxWatermark w kontrolce FormView (VB)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark1.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark1VB.pdf)

> Kontrolka używanie textboxwatermark w zestawie narzędzi AJAX Control rozszerza pole tekstowe tak, aby tekst był wyświetlany w polu. Gdy użytkownik kliknie pole, zostanie opróżnione. Jeśli użytkownik opuści pole bez wprowadzania tekstu, zostanie wyświetlony tekst wstępnie wypełniony. Jest to również możliwe w kontrolce FormView.

## <a name="overview"></a>Omówienie

Kontrolka `TextBoxWatermark` w zestawie narzędzi AJAX Control rozszerza pole tekstowe tak, aby tekst był wyświetlany w polu. Gdy użytkownik kliknie pole, zostanie opróżnione. Jeśli użytkownik opuści pole bez wprowadzania tekstu, zostanie wyświetlony tekst wstępnie wypełniony. Jest to również możliwe w ramach formantu `FormView`.

## <a name="steps"></a>Kroki

Najpierw wymagane jest źródło danych. Ten przykład używa bazy danych AdventureWorks i Microsoft SQL Server 2005 Express Edition. Baza danych jest opcjonalną częścią instalacji programu Visual Studio (w tym Express Edition) i jest również dostępna jako oddzielne pobieranie w obszarze [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). Baza danych AdventureWorks jest częścią przykładów SQL Server 2005 i przykładowych baz danych (Pobierz w [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;D isplaylang = EN](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Najprostszym sposobem ustawienia bazy danych jest użycie Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = EN](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) i dołączenie pliku bazy danych `AdventureWorks.mdf`.

W tym przykładzie przyjęto założenie, że wystąpienie SQL Server 2005 Express Edition jest nazywane `SQLEXPRESS` i znajduje się na tym samym komputerze co serwer sieci Web; jest to również konfiguracja domyślna. Jeśli konfiguracja jest inna, należy dostosować informacje o połączeniu dla bazy danych.

Aby aktywować funkcje ASP.NET AJAX i zestawu narzędzi sterowania, formant `ScriptManager` musi być umieszczony w dowolnym miejscu na stronie (ale w obrębie elementu `<form>`):

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample1.aspx)]

Następnie Dodaj źródło danych do strony, która obsługuje instrukcje SQL `DELETE`, `INSERT` i `UPDATE`. Jeśli używasz asystenta programu Visual Studio do utworzenia źródła danych, weź pod uwagę, że usterka w bieżącej wersji nie zawiera prefiksu nazwy tabeli (`Vendor`) z `Purchasing`. Następujące znaczniki pokazują poprawną składnię:

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample2.aspx)]

Zapamiętaj nazwę (`ID`) źródła danych, ponieważ będzie on używany we właściwości `DataSourceID` kontrolki `FormView`. Sekcja `<InsertItemTemplate>` `FormView` zawiera pole tekstowe, które jest rozszerzane przez kontrolkę `TextBoxWatermarkExtender`. Upewnij się, że rozszerzenie znajduje się w szablonie, a nie poza nim.

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample3.aspx)]

Teraz, gdy użytkownik zmieni tryb wstawiania kontrolki `FormView`, pole tekstowe nowego dostawcy jest wypełniane za pomocą kontrolki `TextBoxWatermarkExtender`. Kliknięcie wewnątrz pola tekstowego umożliwia zniknięcie tekstu Filler.

[![znak wodny w polu pochodzi z rozszerzenia](using-textboxwatermark-in-a-formview-vb/_static/image2.png)](using-textboxwatermark-in-a-formview-vb/_static/image1.png)

Znak wodny w polu pochodzi z rozszerzenia (kliknij,[Aby wyświetlić obraz o pełnym rozmiarze](using-textboxwatermark-in-a-formview-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](using-textboxwatermark-with-validation-controls-cs.md)
> [dalej](using-textboxwatermark-with-validation-controls-vb.md)
